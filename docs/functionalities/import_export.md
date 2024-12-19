## Sylius B2B Kit

Sylius B2B Kit is a dedicated B2B solution. It's a bundle of already configured features that allows you to quickly build B2B experience.

More details about features can be found [here](../functionalities.md)

---

### Import/Export

Allows importing and exporting products, orders, customers, etc. It is a developer friendly feature, which allows you to easily create your own importers and exporters. Which can significantly speed up your migration process.

---

### Developer Guide

#### Available commands

So far `csv`, `tsv` and `rabbit_mq` input types are supported.

#### Import

```bash
$ php bin/console sylius:import:csv <resource-code> <csv-file-path> [<batch-size>] # Imports a resource from a CSV file
$ php bin/console sylius:import:sql <resource-code> <query-string> [<batch-size>] # Imports a resource using provided SQL query
```

Note: `batch-size` is an optional parameter which can improve performance for large imports.

Default delimiter (which is a comma `,`) can be changed with the `--delimiter` option.

#### Export

```bash
$ php bin/console sylius:export <output-format> [<resource-code>] # Exports a resource (or group of resources) to a specific format
```


#### Adding a new importer

1. Create the following interface and class:

```php
<?php

declare(strict_types=1);

namespace App\Importer;

use Sylius\B2BKit\Importer\ImporterInterface;

interface BrandImporterInterface extends ImporterInterface
{
    public const CODE_COLUMN = 'code';
    public const NAME_COLUMN = 'name';
}
```

```php
<?php

declare(strict_types=1);

namespace App\Importer;

use App\Entity\BrandInterface;
use Sylius\B2BKit\Importer\AbstractImporter;
use Sylius\B2BKit\Resolver\ResourceResolverInterface;
use Sylius\Component\Resource\Model\ResourceInterface;

final class BrandImporter extends AbstractImporter implements BrandImporterInterface
{
    /** @var ResourceResolverInterface */
    private $brandResourceResolver;

    public function __construct(ResourceResolverInterface $brandResourceResolver)
    {
        $this->brandResourceResolver = $brandResourceResolver;
    }

    public function import(array $row): ResourceInterface
    {
        $code = $this->getColumnValue(self::CODE_COLUMN, $row);
        
        /** @var BrandInterface $brand */
        $brand = $this->brandResourceResolver->resolveResource($code);
        $brand->setCode($code);
        $brand->setName($this->getColumnValue(self::NAME_COLUMN, $row));

        return $brand;
    }

    public function getResourceCode(): string
    {
        return 'brand';
    }
}
```

2. Register importer & resource resolver services:

```yaml
services:
    ...

    app.importer.brand:
        class: App\Importer\BrandImporter
        arguments:
            - "@app.resolver.import_resource.brand"
        tags:
            - { name: sylius.importer }

    app.resolver.import_resource.brand:
        class: Sylius\B2BKit\Resolver\ResourceResolver
        arguments:
        - "@app.repository.brand"
        - "@app.factory.brand"
        - "code"
```

3. Create a following sample brand.csv file:

```csv
code,name
2283eed4779c78951b7b27ca61d3ffec,"Johnson & Johnson"
2d1be7d3f051561a9dee88f2999f5734,Appenzeller-Kontaktlinsen
eba833281ed1108cb7005d45654e835b,"Alcon | Ciba Vision"
ee498f9f5cd01cd29b45688b8c53d5a6,"Cooper Vision"
```

4. Run the following command:

```bash
$ bin/console sylius:import:csv brand brand.csv
```

### Adding a new exporter

1. Create the following interface & class:

```php
<?php
   
declare(strict_types=1);

namespace App\Exporter;

use Sylius\B2BKit\Exporter\ExporterInterface;

interface BrandExporterInterface extends ExporterInterface
{
   public const CODE_COLUMN = 'code';
   public const NAME_COLUMN = 'name';
}
```

```php
<?php

declare(strict_types=1);

namespace App\Exporter;

use App\Entity\BrandInterface;
use Sylius\B2BKit\Exporter\AbstractExporter;
use Sylius\Component\Resource\Model\ResourceInterface;
use Sylius\Component\Resource\Repository\RepositoryInterface;

final class BrandExporter extends AbstractExporter implements BrandExporterInterface
{
    /** @var RepositoryInterface */
    private $brandRepository;

    public function __construct(RepositoryInterface $brandRepository)
    {
        $this->brandRepository = $brandRepository;
    }

    public function export(ResourceInterface $resource = null): array
    {
        $data = [];
        $data[] = $this->getColumns();

        if ($resource instanceof BrandInterface) {
            $data[] = $this->exportRow($resource);

            return $data;
        }

        $brands = $this->brandRepository->findAll();

        /** @var BrandInterface $brand */
        foreach ($brands as $brand) {
            $data[] = $this->exportRow($brand);
        }

        return $data;
    }

    public function getResourceCode(): string
    {
        return 'brand';
    }

    public function getColumns(): array
    {
        return [
            self::CODE_COLUMN,
            self::NAME_COLUMN,
        ];
    }

    private function exportRow(BrandInterface $brand): array
    {
        return [
            self::CODE_COLUMN => $brand->getCode(),
            self::NAME_COLUMN => $brand->getName(),
        ];
    }
}
```

2. Register a new exporter service:

```yaml
services:
    ...
    app.exporter.brand:
        class: App\Exporter\BrandExporter
        arguments:
            - "@app.repository.brand"
        tags:
            - { name: sylius.exporter, resourceClass: "%app.model.brand.class%", priority: 5 }

```

3. Run the following command:

```bash
$ bin/console sylius:export csv brand
```

A new file under `var/export/brand.csv` path should be now created.
