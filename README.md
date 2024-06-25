### This package is in development stage, please do not use it in production

### install
```bash
composer require joe.szeto/tablelite
```

update your ```app.php```  file to include the service provider
```php
\Tablelite\TablesliteServiceProvider::class
```


### Usage

```php
use Tablelite\Table as TableAlias;

class SomeComponent extends Component  {
    use \Tablelite\InteractsWithTablelite;
    
    public function table( TableAlias $table ){
        $table->schema([
            TextColumn::make('id'),
            TextColumn::make('name'),
        ])->records([
            [ 'id' => 1, 'name' => 'John'],
            [ 'id' => 2, 'name' => 'Doe'],
        ]);
    }
}
```

in view

```bladehtml

<div>
    {{ $this->getTable() }}
</div>
```

#### KeyBy
if ```id``` not found in the records, you can use the ```keyBy``` method to specify the key
```php
$table->keyBy('some_key');
```

#### Actions
```php
 $table->schema([
    TextColumn::make('id'),
    TextColumn::make('name'),
])
    ->records([
        ['id' => 1, 'name' => 'John'],
        ['id' => 2, 'name' => 'Doe'],
    ])->actions(fn(ActionFactory $actionFactory) => [
        $actionFactory->make('some_action')
            ->label('Action Label')
            ->action(function ($record) {
                // do something here
             })
    ]);

```
Text style action button
```php
$actionFactory->make('some_action')->text()
```

#### header actions
```php
 ->headerActions(fn(ActionFactory $actionFactory) => [
    $actionFactory->make('assign')
        ->label('Assign Coupon')
        ->slideover('some-livewire-component', [
            // params for the livewire component...
        ])
])
```

#### Slide Over
use ``` IsSlideOver ``` in your livewire component
when we want to do something when slide over open,
we can use the ```onSlideOver``` method
```php
$this->onSlideOver(
    'someMethod',
    loading: true,
    loadingPattern: 'cccb|accc'
)
```

#### pagination
```php
$table->schema([
    TextColumn::make('id'),
    TextColumn::make('name'),
])
    ->records(
        function ($page) {
            // fetch api here
        }
    )
    ->paginateUsing(
        function ($records, PaginatorBuilder $builder) {
            // records is the response of the fetching
            return $builder
                ->items($records['data']) // the items
                ->total($records['total']) // total items
                ->perPage($records['per_page'])
                ->currentPage($records['current_page']);
        }
    );
```

### Development in local

#### clone the repository to your local machine

#### add this to your composer.json file

```json
"repositories": [
    {
        "type": "path",
        "url": "path/to/tablelite"
    }
]
```

make sure update the path to the correct path

#### run
```php
composer require joe.szeto/tablelite
```
