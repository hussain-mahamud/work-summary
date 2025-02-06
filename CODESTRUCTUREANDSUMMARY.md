# SaaS Human Resource Management System (HRM) – Ongoing Development

I am currently working on a SaaS-based Human Resource Management System (not yet live), where I have contributed to multiple core modules. Due to confidentiality, I am unable to share repository links or demo access, but I can provide insights into my contributions.

## Builder Package Development

One of my key responsibilities was developing a Builder Package that allows junior engineers to create and manage various components with minimal effort. This package significantly reduces development time by enabling engineers to define attributes with a few lines of code, and the system automatically generates the necessary fields, validation rules, and frontend components.

### Implementation:

```php
class ExampleModel extends OurBaseModel
{
    public function setDataAttributes()
    {
        return [

            'name' => [
                'type' => self::TEXT, // Basic type of the attribute in frontend a new field is created
                'label' => 'Name', // Label of the attribute in frontend
                'validation' => [
                    'default' => 'sometimes|required|regex:/^[a-zA-Z0-9-_ ]+$/', // Validation rules in backend
                    'required_if' => ['type', '$eq', 'folder'],
                ],
                'sanitization' => 'trim', // Sanitization rules in backend
                'props' => [
                    'placeholder' => 'Name', // Props of the attribute in frontend

                ],
            ],
            'document_tag_ids'    => [
                'type'       => self::SELECT, // Basic type of the attribute in frontend a new select field is created
                'label'      => 'Tags',
                'filter'     => true, // That means this attribute can be used in filter and you filter it
                'validation' => [
                    'default' => 'nullable|array',
                ],
                'props'      => [
                    'maxCount'     => 5, // That means maximum 5 tags can be selected
                    "mode"         => 'multiple', // That means multiple tags can be selected
                    "searchColumn" => "name",
                    "searchLabel"  => "_id",
                    'optionsApi'   => ['modules.documents.document-tag.list', 'advanced-filter.search'], // That means options of the select field are coming from this api
                    'page'         => [
                        'href'   => 'modules.documents.document-tag.form', // That means from the option if you select add new then you can create new tags from this api
                    ],
                    'placeholder'  => 'Select Tags',
                ],
                'sortable'   => false, // That means tags can be sorted in table
            ],
            
}
```

#### Form & Table Component

in form you can easily pass fields and pass the form design also by just providing a few lines of code.
```php
 class FormComponent extends OurBaseFormComponent
{
    public static $componentModel = OurModel::class;
    public static $structureTitle = ['Add', 'Edit', 'Details'];  // form title
    public static $structureIcon = 'Profile'; // form icon
    public static $componentRouteName = 'form-component';
    public static $actionForm = ['modules.documents.document-tag.form', 'form-component']; // form action api

    protected function buildForm(): void
    {
        Form::collect(
            Form::rowWithSingleColumn(
                Form::input('name')
            ),
            Form::rowWithTwoColumns(
                Form::input('color')
            )
        );
    }
}
```
In Table you can easily pass columns and pass the table design also by just providing a few lines of code.

```php
class TableComponent extends OurBaseTableComponent
{
    public static $componentModel = OurModel::class;
    public static $globalSearchAttributes = ['name']; // That means you can search by name in table search box
    public static $componentRouteName = 'table-component';
    public static $actionForm = ['modules.documents.document-tag.form', 'form-component',]; // table action api
    public static $structureTitle = 'List of Document Tags'; // table title

    protected function rebuildStructure($structure)
    {
        return $structure->actionItems(
            Build::actionItem(api: ['default' => URL::component([static::$actionForm[0], static::$actionForm[1] . '.delete-content'], $this->setQueryParams())], type: 'delete'),
            Build::actionItem(
                page: Build::page(
                    target: "_popup",
                    href: static::$actionForm[0],
                ),
                type: 'create'
            ),
        );
    }

    protected function buildTable(): void
    {
        Table::collect(

            Table::column('name')
            ->transform(
                type: 'joinByCommaWithLink', // That means if you get result multiple then you can join them by comma and 
                linkEndpoint: URL::page('modules.documents.document-tag.form'), //if you click on them you can go to the details page
                with: Table::transformWith(_id: '_id', mainText: 'name'),
                target: "_popup", // That means if you click it will be opened on sidebar
            ),
            
            Table::column('color'),

// Below column action will be on each row
            Table::columnActionItem(
                icon: 'ViewOnlyLiner',
                page: Build::page(
                    target: "_popup",
                    href: static::$actionForm[0]
                ),
                type: 'view'
            ),

            Table::columnActionItem(
                icon: 'EditLiner',
                page: Build::page(
                    target: "_popup",
                    href: static::$actionForm[0]
                ),
                type: 'edit'
            ),

            Table::columnActionItem(
                icon: 'Trash01Liner',
                api: ['default' => URL::component([static::$actionForm[0], static::$actionForm[1] . '.delete-content'])],
                type: 'delete'
            ),
        );
    }
}
```

### Custom Notification System

I designed and developed highly flexible Notification System that allows developers to trigger notifications with minimal code.
Usages:
```php
    $employees = ['user_id_1', 'user_id_2']
    $notificationId = Notification::type('card') // that means notification type is card
            ->to($employees) // array of ids
            ->toModel('User') // model name
            ->title('Test Notification') // 
            ->cardUrl(URL::page('modules.announcements.announcement.form', ['formState' => 'view', '_id' => $notification->_id])) // clicking on this notification will open a form. This is the endpoint.formState=view that means link will be open in view mode
            ->description('This is test content')
            ->category('inbox.announcements')
            ->sender('sender_id') //
            ->send(); // that means dispatch the notification job
```
This system handles notifications efficiently using a queue, reducing load on the system.

### Request &Approval Package Development
I designed and developed highly flexible request and approval System that allows developers to trigger request and approval with minimal code.


```php 

RequestRecord::settingName('inbox.approval.leave')
            ->notificationType('card')
            ->requestTitle('I\'m requesting  casual leave of  Feb 06 To Feb 10 Leave')')
            ->handler(['Modules\LeaveAndAttendance\Models\Leave\LeaveModel', $request])
            ->createdBy(User::current()->_id)
            ->requestCard(URL::page('package.settings.job-information-approve'))
            ->create();
```
Besides these modules and packages I have contributed to other core modules as well.
- Feedback Module
- Inventory Module


Before joining my current role, I worked at **PRAN RFL Group**, one of the largest conglomerates in Bangladesh. My primary responsibility was developing and maintaining the Secondary Sales Automation System, which was the backbone of the company's business operations.

### About PRAN RFL Group:
- PRAN Group consists of 20+ subsidiary companies, while RFL Group operates around 17 companies.
- They conduct business across multiple countries and employ approximately 150,000 people.
- PRAN Group distributes products to around 1.2 million retail outlets, while RFL Group serves 700,000+ stalls.
- Large-Scale Data Handling Experience:
PRAN RFL Group's operations meant handling and processing millions of data points daily, including:

- Sales Representative (SR) activity data
- Order and collection data
- Monitoring and attendance records
- Geo-location tracking

*To efficiently process this massive volume of data, I developed APIs and various reports, implementing optimized scheduling mechanisms such as database scheduling and cron jobs. This experience allowed me to gain deep expertise in data processing, system optimization, and performance tuning.*

### Key Modules I Worked On:
- Attendance Module
- Order Module
- Collection Module
- Monitoring Module
- SR Activity Module
- Reporting Module
- Space Management
- TeleSales Module

*Due to confidentiality agreements, I am unable to share code samples or demo access. However, my experience at PRAN RFL Group has significantly strengthened my ability to build scalable, high-performance systems that can handle complex business operations efficiently*