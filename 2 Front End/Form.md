# Form

`$ composer require symfony/form`

Goals of a Form
1. Translate data from object -> HTML form so a user can modify that data
2. Take submitted data and re-apply it to the object

Symfony handles
- rendering HTML form fields
- validating submitted data
- mapping the form data into objects
- and more

## Form Types

Everything is a "form type"

- forms
- form fields

### Reference

<div class="section">
    <h4 id="text-fields">Text Fields</h4>
    <ul>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/text.html" class="reference internal">TextType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/textarea.html" class="reference internal">TextareaType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/email.html" class="reference internal">EmailType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/integer.html" class="reference internal">IntegerType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/money.html" class="reference internal">MoneyType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/number.html" class="reference internal">NumberType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/password.html" class="reference internal">PasswordType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/percent.html" class="reference internal">PercentType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/search.html" class="reference internal">SearchType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/url.html" class="reference internal">UrlType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/range.html" class="reference internal">RangeType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/tel.html" class="reference internal">TelType</a></li>
        <li><a href="https://symfony.com/doc/5.4/reference/forms/types/color.html" class="reference internal">ColorType</a></li>
    </ul>
</div>
<div class="section">
<h4 id="choice-fields">Choice Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/choice.html" class="reference internal">ChoiceType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/enum.html" class="reference internal">EnumType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/entity.html" class="reference internal">EntityType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/country.html" class="reference internal">CountryType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/language.html" class="reference internal">LanguageType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/locale.html" class="reference internal">LocaleType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/timezone.html" class="reference internal">TimezoneType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/currency.html" class="reference internal">CurrencyType</a></li>
</ul>
</div>
<div class="section">
<h4 id="date-and-time-fields">Date and Time Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/date.html" class="reference internal">DateType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/dateinterval.html" class="reference internal">DateIntervalType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/datetime.html" class="reference internal">DateTimeType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/time.html" class="reference internal">TimeType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/birthday.html" class="reference internal">BirthdayType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/week.html" class="reference internal">WeekType</a></li>
</ul>
</div>
<div class="section">
<h4 id="other-fields">Other Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/checkbox.html" class="reference internal">CheckboxType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/file.html" class="reference internal">FileType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/radio.html" class="reference internal">RadioType</a></li>
</ul>
</div>
<div class="section">
<h4 id="symfony-ux-fields">Symfony UX Fields</h4>
<p>These types are part of the <a href="https://symfony.com/doc/5.4/frontend/ux.html" class="reference internal">Symfony UX initiative</a>:</p>
<ul>
    <li><a href="https://github.com/symfony/ux/tree/2.x/src/Cropperjs#readme" class="reference external" rel="external noopener noreferrer" target="_blank">CropperType</a> (using Cropper.js)</li>
    <li><a href="https://github.com/symfony/ux/tree/2.x/src/Dropzone#readme" class="reference external" rel="external noopener noreferrer" target="_blank">DropzoneType</a></li>
</ul>
</div>
<div class="section">
<h4 id="uid-fields">UID Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/uuid.html" class="reference internal">UuidType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/ulid.html" class="reference internal">UlidType</a></li>
</ul>
</div>
<div class="section">
<h4 id="field-groups">Field Groups</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/collection.html" class="reference internal">CollectionType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/repeated.html" class="reference internal">RepeatedType</a></li>
</ul>
</div>
<div class="section">
<h4 id="hidden-fields">Hidden Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/hidden.html" class="reference internal">HiddenType</a></li>
</ul>
</div>
<div class="section">
<h4 id="buttons">Buttons</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/button.html" class="reference internal">ButtonType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/reset.html" class="reference internal">ResetType</a></li>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/submit.html" class="reference internal">SubmitType</a></li>
</ul>
</div>
<div class="section">
<h4 id="base-fields">Base Fields</h4>
<ul>
    <li><a href="https://symfony.com/doc/5.4/reference/forms/types/form.html" class="reference internal">FormType</a></li>
</ul>
</div>

### [Create a Form Type](https://symfony.com/doc/5.4/form/create_custom_field_type.html)

## Workflow

1. Build the form
   - in a Symfony controller
   - or a dedicated form class
   - or hand write the twig file
2. Render the form
   - create a template where the user can edit and submit data
   - take data from an object and plug it into the HTML form to fill fields
   - Symfony is smart enough to access private and protected fields of an Entity
     - For a regular field, it will use `getTask()`
     - For a Boolean field, it can use *isser* or *hasser* (`isPublished()` or `hasReminder`)
3. Process the form
   - validate the submitted data
   - transform into PHP data
   - do something with it (persist in db)

## Build Form

Symfony provides a "form builder" that creates a form object, uses it to render, and process the contents

- Tips:
  - Include / use the namespace of all field types used

### In Controllers

(not recommended, keep controllers slim)

If controller extends `AbstractController`, 
- use `createFormBuilder()` helper,
- else fetch the `form.factory` service and use `createBuilder()`

```php
// src/Controller/TaskController.php
namespace App\Controller;

use App\Entity\Task;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Form\Extension\Core\Type\DateType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

class TaskController extends AbstractController
{
    public function new(Request $request): Response
    {
        // creates a task object and initializes some data for this example
        $task = new Task();
        $task->setTask('Write a blog post');
        $task->setDueDate(new \DateTime('tomorrow'));

        // build form
        $form = $this->createFormBuilder($task) //auto connects $task->task and $task->dueDate
            ->setAction($this->generateUrl('target_route')) //default path is this controller
            ->setMethod('GET') //default POST
            ->add('task', TextType::class)
            ->add('dueDate', DateType::class)
            ->add('save', SubmitType::class, ['label' => 'Create Task'])
            ->getForm();

        // render

    }
}
```

### In Form Classes

(recommended - can be reused in multiple actions and services)

A form class is a form type that implements `FormTypeInterface` or extends `AbstractType` (better, implements the interface and provides extra utilities)

1. Create class (`TaskType.php`)
   1. `use Symfony\Component\Form\AbstractType;`
   2. import all used field types
      - `use Symfony\Component\Form\Extension\Core\Type\DateType;`
   3. `class TaskType extends AbstractType`
   4. Make `buildForm()`
      ```php
      public function buildForm(FormBuilderInterface $builder, array $options): void
      {
          $builder
              ->add('task', TextType::class)
              ->add('dueDate', DateType::class)
              ->add('save', SubmitType::class)
          ;
      }
      ```
2. Use in controller
   1. `use App\Form\Type\TaskType;`
   2. `$form = $this->createForm(TaskType::class, $task)`
      - if the controller doesnt extend `AbstractController`, use `create()` from `form.factory` service
      - The form needs to know what holds the data (`createForm()` 2nd arg)
        - `$task` is good for now but not good for [embedding forms](https://symfony.com/doc/5.4/form/embedded.html) / using multiple entities
   3. (optional) Set the HTTP action and method
      ```php
      $form = $this->createForm(TaskType::class, $task, [
            'action' => $this->generateUrl('target_route'),
            'method' => 'GET',
        ]);
      ```
3. (optional) Explicitly specify options in `configureOptions()`

#### Generate Form Class

`$ php bin/console make:form`

`$ php bin/console make:registration-form`

### Form Options

Helps the form be built on the back end by specifying where data comes from for inserting into the fields and custom options

- `data_class`
  - the `data_class` option defines where data comes from (useful when more than one entity used)
    ```php

    // src/Form/Type/TaskType.php
    namespace App\Form\Type;

    use App\Entity\Task;
    use Symfony\Component\OptionsResolver\OptionsResolver; #add
    // ...

    class TaskType extends AbstractType
    {
        // ...

        #add
        public function configureOptions(OptionsResolver $resolver): void
        {
            $resolver->setDefaults([
                'data_class' => Task::class,
            ]);
        }
    }
    ```
  - This option allows you to use multiple entities (i.e. embed forms)
    - [More about embedding forms](https://symfony.com/doc/5.4/form/embedded.html)
- custom options
  - the custom options passed in as the 3rd argument of `createForm()` MUST be declared in the `configureOptions()` method
    ```php
    //controller
    $form = $this->createForm(TaskType::class, $task, [
        'require_due_date' => true, 
        //...
    ]);

    //TaskType
    use Symfony\Component\OptionsResolver\OptionsResolver;

    public function configureOptions(OptionsResolver $resolver): void
    {
        $resolver->setDefaults([
            //...
            'require_due_date' => false,
        ]);
        // you can also define the allowed types, allowed values and
        // any other feature supported by the OptionsResolver component
        $resolver->setAllowedTypes('require_due_date', 'bool');
    }
    //TaskType (now you can use the option on fields)
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            // ...
            ->add('dueDate', DateType::class, [
                'required' => $options['require_due_date'],
            ])
        ;
    }
    ```

### Field Options

Only affects how the form looks and interacts with the user (front end), doesnt touch back end

Affects the html tag attributes like:

#### required

By default set to `true`

Doesnt do any server-side validation, if a blank value makes it through submission it will be valid. `NotBlank` or `NotNull` validation constraints do the server-side validation

#### label

By default set to the humanized version of th property name

Styling:
- label of required fields have a "required" class

#### attr

`->add('task', null, ['attr' => ['maxlength' => 4]])`

#### placeholder

#### data

## Render Form

In the controller take the built form and plug it into `renderForm()` and return the results

```php
return $this->renderForm('task/new.html.twig', [
    'form'=> $form,
]);
```

Then in the template render the form contents using the [form helper functions](https://symfony.com/doc/5.4/form/form_customization.html#reference-form-twig-functions)

```twig
{# templates/task/new.html.twig #}
{{ form(form) }}
```

### [{{ form(form) }}](https://symfony.com/doc/5.4/form/form_customization.html#reference-forms-twig-form)

- renders all fields and the `<form>` tags
- method is `POST` by default
  - `{{ form_start(form, {'action': path('target_route'), 'method': 'GET'}) }}`
  - If method is not GET or POST, but instead PUT, PATCH, OR DELETE
    - Sends via POST but with a hidden field called `_method` which Symfony routing will look for when deciding which HTTP method was used
    - Requires [Framework Configuration Reference](https://symfony.com/doc/5.4/reference/configuration/framework.html#configuration-framework-http_method_override) to be enabled
- target URL is the same that displayed the form
  - `{{ form_start(form, {'action': path('target_route'), 'method': 'GET'}) }}`
- [this can all be change](https://symfony.com/doc/5.4/forms.html#forms-change-action-method)

### render() vs renderForm()

In Symfony 5.2 and below, `$this->render('task/new.html.twig', ['form' => $form->createView()]);` was used to render the form

In Symfony 5.3, `$this->renderForm('task/new.html.twig', ['form' => $form]);` started to be used.

- `renderView()` abstracts the logic and sets the 422 HTTP status code in the Response when the submitted form is not valid

## Process Form

Use single action to render the form and handle the form submission, like in a controller (*recommended*)

1. `$form->handleRequest($request);`
   - recognizes the form submission
   - write sumbitted data into form object AND entity
   - if no object was supplied to `createFormBuilder()` or to the `data_class` option on a form, it will return as an array
2. `if ($form->isSubmitted() && $form->isValid())`
   - `isSubmitted()`
     - check if the form is in a submitted state
   - `isValid()`
     - checks the `$task` object if it has valid data
       - if invalid it renders the form again but with validation errors
3. `$task = $form->getData();`
   - translate submitted data to properties of entity (if object was supplied)
   - better than `$request->request->get('name')` because getData uses the Form component to return the data
4. `return $this->redirectToRoute('task_success');`
   - redirecting a user after a successful form submission is best practice to prevent a "Refresh" from re-posting the data

### Submit() | [Control Over Form Submission](https://symfony.com/doc/5.4/form/direct_submit.html)

Control when form is submitted and which data is passed to it

#### [How to handle form submissions](https://symfony.com/doc/5.4/form/direct_submit.html)

Better to use [handleRequest()](https://github.com/symfony/symfony/blob/5.4/src/Symfony/Component/Form/FormInterface.php#:~:text=function%20handleRequest) but [submit()](https://github.com/symfony/symfony/blob/5.4/src/Symfony/Component/Form/FormInterface.php#:~:text=function%20submit) can also be used to have better control over WHEN exactly your form is submitted and WHAT data is passed to it

### Validation Constraints | [Validating Forms](https://symfony.com/doc/5.4/validation.html)

> The question isnt if the "form" is valid, but if the underlying object / entity is valid

Install
`$ composer require symfony/validator`

Validation is done by 
1. adding a set of rules / **validation constraints** to
   - an entity class
   - or to form types / field types by using the [constraints option](https://symfony.com/doc/5.4/form/without_class.html#adding-validation)
   - or both
2. Calling the `validate()` service

#### Validation Constraints

P.S. need to import / use the namespace of the constraints used

##### Validation Constraints on Entity

Add validation constraints so that

1. `task` cannot be empty
2. `dueDate` cannot be empty and is a valid `DateTime` object

yaml config

```yaml
# config/validator/validation.yaml
App\Entity\Task:
    properties:
        task:
            - NotBlank: ~
        dueDate:
            - NotBlank: ~
            - Type: \DateTime
```

php controller annotations

```php
// src/Entity/Task.php
namespace App\Entity;

use Symfony\Component\Validator\Constraints as Assert;

class Task
{
    /**
     * @Assert\NotBlank
     */
    public $task;

    /**
     * @Assert\NotBlank
     * @Assert\Type("\DateTime")
     */
    protected $dueDate;
}
```

##### Validation Constraints on Fields

Set the constraints on individual fields

```php
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Validator\Constraints\Length;
use Symfony\Component\Validator\Constraints\NotBlank;

public function buildForm(FormBuilderInterface $builder, array $options): void
{
    $builder
        ->add('firstName', TextType::class, [
            'constraints' => new Length(['min' => 3]),
        ])
        ->add('lastName', TextType::class, [
            'constraints' => [
                new NotBlank(),
                new Length(['min' => 3]),
            ],
        ])
    ;
}
```

##### Validation Constraints at Form Class Level

Set the constraints at the class level

```php
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Validator\Constraints\Length;
use Symfony\Component\Validator\Constraints\NotBlank;

public function buildForm(FormBuilderInterface $builder, array $options): void
{
    $builder
        ->add('firstName', TextType::class)
        ->add('lastName', TextType::class);
}

public function configureOptions(OptionsResolver $resolver): void
{
    $constraints = [
        'firstName' => new Length(['min' => 3]),
        'lastName' => [
            new NotBlank(),
            new Length(['min' => 3]),
        ],
    ];

    $resolver->setDefaults([
        'data_class' => null,
        'constraints' => $constraints,
    ]);
}
```

This means that you can do this for `createFormBuilder()`:
```php
$form = $this->createFormBuilder($defaultData, [
        'constraints' => [
            'firstName' => new Length(['min' => 3]),
            'lastName' => [
                new NotBlank(),
                new Length(['min' => 3]),
            ],
        ],
    ])
    ->add('firstName', TextType::class)
    ->add('lastName', TextType::class)
    ->getForm();
```

#### Validate()

Setting constraints wont actually validate, you have to pass it to the validator service

```php
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Validator\Validator\ValidatorInterface;

$errors = $validator->validate($myObject);
if(count($errors) > 0){
    $errorsString = (string) $errors;
    return new Response ($errorsString);
}
return new Response('The object is valid!');
```

#### Constraints List

<div class="section">
    <h5 id="basic-constraints"><a class="headerlink" href="#basic-constraints" title="Permalink to this headline">Basic Constraints</a></h5>
    <p>These are the basic constraints: use them to assert very basic things about the value of properties or the return value of methods on your object.</p>
    <ul>
        <li><a href="reference/constraints/NotBlank.html" class="reference internal">NotBlank</a></li>
        <li><a href="reference/constraints/Blank.html" class="reference internal">Blank</a></li>
        <li><a href="reference/constraints/NotNull.html" class="reference internal">NotNull</a></li>
        <li><a href="reference/constraints/IsNull.html" class="reference internal">IsNull</a></li>
        <li><a href="reference/constraints/IsTrue.html" class="reference internal">IsTrue</a></li>
        <li><a href="reference/constraints/IsFalse.html" class="reference internal">IsFalse</a></li>
        <li><a href="reference/constraints/Type.html" class="reference internal">Type</a></li>
    </ul>
</div>
<div class="section">
    <h5 id="string-constraints"><a class="headerlink" href="#string-constraints" title="Permalink to this headline">String Constraints</a></h5>
    <ul>
        <li><a href="reference/constraints/Email.html" class="reference internal">Email</a></li>
        <li><a href="reference/constraints/ExpressionLanguageSyntax.html" class="reference internal">ExpressionLanguageSyntax</a></li>
        <li><a href="reference/constraints/Length.html" class="reference internal">Length</a></li>
        <li><a href="reference/constraints/Url.html" class="reference internal">Url</a></li>
        <li><a href="reference/constraints/Regex.html" class="reference internal">Regex</a></li>
        <li><a href="reference/constraints/Hostname.html" class="reference internal">Hostname</a></li>
        <li><a href="reference/constraints/Ip.html" class="reference internal">Ip</a></li>
        <li><a href="reference/constraints/Cidr.html" class="reference internal">Cidr</a></li>
        <li><a href="reference/constraints/Json.html" class="reference internal">Json</a></li>
        <li><a href="reference/constraints/Uuid.html" class="reference internal">Uuid</a></li>
        <li><a href="reference/constraints/Ulid.html" class="reference internal">Ulid</a></li>
        <li><a href="reference/constraints/UserPassword.html" class="reference internal">UserPassword</a></li>
        <li><a href="reference/constraints/NotCompromisedPassword.html" class="reference internal">NotCompromisedPassword</a></li>
        <li><a href="reference/constraints/CssColor.html" class="reference internal">CssColor</a></li>
    </ul>
</div>
<div class="section">
    <h5 id="comparison-constraints"><a class="headerlink" href="#comparison-constraints" title="Permalink to this headline">Comparison Constraints</a></h5>
    <ul>
        <li><a href="reference/constraints/EqualTo.html" class="reference internal">EqualTo</a></li>
<li><a href="reference/constraints/NotEqualTo.html" class="reference internal">NotEqualTo</a></li>
<li><a href="reference/constraints/IdenticalTo.html" class="reference internal">IdenticalTo</a></li>
<li><a href="reference/constraints/NotIdenticalTo.html" class="reference internal">NotIdenticalTo</a></li>
<li><a href="reference/constraints/LessThan.html" class="reference internal">LessThan</a></li>
<li><a href="reference/constraints/LessThanOrEqual.html" class="reference internal">LessThanOrEqual</a></li>
<li><a href="reference/constraints/GreaterThan.html" class="reference internal">GreaterThan</a></li>
<li><a href="reference/constraints/GreaterThanOrEqual.html" class="reference internal">GreaterThanOrEqual</a></li>
<li><a href="reference/constraints/Range.html" class="reference internal">Range</a></li>
<li><a href="reference/constraints/DivisibleBy.html" class="reference internal">DivisibleBy</a></li>
<li><a href="reference/constraints/Unique.html" class="reference internal">Unique</a></li>
</ul>
</div>
<div class="section">
<h5 id="number-constraints"><a class="headerlink" href="#number-constraints" title="Permalink to this headline">Number Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/Positive.html" class="reference internal">Positive</a></li>
<li><a href="reference/constraints/PositiveOrZero.html" class="reference internal">PositiveOrZero</a></li>
<li><a href="reference/constraints/Negative.html" class="reference internal">Negative</a></li>
<li><a href="reference/constraints/NegativeOrZero.html" class="reference internal">NegativeOrZero</a></li>
</ul>
</div>
<div class="section">
<h5 id="date-constraints"><a class="headerlink" href="#date-constraints" title="Permalink to this headline">Date Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/Date.html" class="reference internal">Date</a></li>
<li><a href="reference/constraints/DateTime.html" class="reference internal">DateTime</a></li>
<li><a href="reference/constraints/Time.html" class="reference internal">Time</a></li>
<li><a href="reference/constraints/Timezone.html" class="reference internal">Timezone</a></li>
</ul>
</div>
<div class="section">
<h5 id="choice-constraints"><a class="headerlink" href="#choice-constraints" title="Permalink to this headline">Choice Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/Choice.html" class="reference internal">Choice</a></li>
<li><a href="reference/constraints/Language.html" class="reference internal">Language</a></li>
<li><a href="reference/constraints/Locale.html" class="reference internal">Locale</a></li>
<li><a href="reference/constraints/Country.html" class="reference internal">Country</a></li>
</ul>
</div>
<div class="section">
<h5 id="file-constraints"><a class="headerlink" href="#file-constraints" title="Permalink to this headline">File Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/File.html" class="reference internal">File</a></li>
<li><a href="reference/constraints/Image.html" class="reference internal">Image</a></li>
</ul>
</div>
<div class="section">
<h5 id="financial-and-other-number-constraints"><a class="headerlink" href="#financial-and-other-number-constraints" title="Permalink to this headline">Financial and other Number Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/Bic.html" class="reference internal">Bic</a></li>
<li><a href="reference/constraints/CardScheme.html" class="reference internal">CardScheme</a></li>
<li><a href="reference/constraints/Currency.html" class="reference internal">Currency</a></li>
<li><a href="reference/constraints/Luhn.html" class="reference internal">Luhn</a></li>
<li><a href="reference/constraints/Iban.html" class="reference internal">Iban</a></li>
<li><a href="reference/constraints/Isbn.html" class="reference internal">Isbn</a></li>
<li><a href="reference/constraints/Issn.html" class="reference internal">Issn</a></li>
<li><a href="reference/constraints/Isin.html" class="reference internal">Isin</a></li>
</ul>
</div>
<div class="section">
<h5 id="other-constraints"><a class="headerlink" href="#other-constraints" title="Permalink to this headline">Other Constraints</a></h5>
<ul>
    <li><a href="reference/constraints/AtLeastOneOf.html" class="reference internal">AtLeastOneOf</a></li>
    <li><a href="reference/constraints/Sequentially.html" class="reference internal">Sequentially</a></li>
    <li><a href="reference/constraints/Compound.html" class="reference internal">Compound</a></li>
    <li><a href="reference/constraints/Callback.html" class="reference internal">Callback</a></li>
    <li><a href="reference/constraints/Expression.html" class="reference internal">Expression</a></li>
    <li><a href="reference/constraints/All.html" class="reference internal">All</a></li>
    <li><a href="reference/constraints/Valid.html" class="reference internal">Valid</a></li>
    <li><a href="reference/constraints/Cascade.html" class="reference internal">Cascade</a></li>
    <li><a href="reference/constraints/Traverse.html" class="reference internal">Traverse</a></li>
    <li><a href="reference/constraints/Collection.html" class="reference internal">Collection</a></li>
    <li><a href="reference/constraints/Count.html" class="reference internal">Count</a></li>
    <li><a href="reference/constraints/UniqueEntity.html" class="reference internal">UniqueEntity</a></li>
    <li><a href="reference/constraints/EnableAutoMapping.html" class="reference internal">EnableAutoMapping</a></li>
    <li><a href="reference/constraints/DisableAutoMapping.html" class="reference internal">DisableAutoMapping</a></li>
</ul>
<p>You can also create your own custom constraints. This topic is covered in
the <a href="validation/custom_constraint.html" class="reference internal">How to Create a Custom Validation Constraint</a> article.</p>
<span id="validation-constraint-configuration"></span>
</div>

## Example

A to do list application that display tasks

Users create and edit tasks using Symfony forms

### Task Object

```php
// src/Entity/Task.php
namespace App\Entity;

class Task
{
    protected $task;
    protected $dueDate; //optional

    public function getTask(): string
    {
        return $this->task;
    }

    public function setTask(string $task): void
    {
        $this->task = $task;
    }

    public function getDueDate(): ?\DateTime
    {
        return $this->dueDate;
    }

    public function setDueDate(?\DateTime $dueDate): void
    {
        $this->dueDate = $dueDate;
    }
}
```

### Build Form & Render Example (Controller)

```php
// src/Controller/TaskController.php
namespace App\Controller;

use App\Entity\Task;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Form\Extension\Core\Type\DateType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;

class TaskController extends AbstractController
{
    public function new(Request $request): Response
    {
        // creates a task object and initializes some data for this example
        $task = new Task();
        $task->setTask('Write a blog post');
        $task->setDueDate(new \DateTime('tomorrow'));

        // build form
        $form = $this->createFormBuilder($task) //auto connects $task->task and $task->dueDate
            ->add('task', TextType::class)
            ->add('dueDate', DateType::class)
            ->add('save', SubmitType::class, ['label' => 'Create Task'])
            ->getForm();

        // render

    }
}
```

### Build Form & Render Example (Form Class)

Build Form

```php
// src/Form/Type/TaskType.php
namespace App\Form\Type;

use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\Extension\Core\Type\DateType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\FormBuilderInterface;

class TaskType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            ->add('task', TextType::class)
            ->add('dueDate', DateType::class)
            ->add('save', SubmitType::class)
        ;
    }
}
```

Then use it in controller

```php
// src/Controller/TaskController.php
namespace App\Controller;

use App\Form\Type\TaskType;
// ...

class TaskController extends AbstractController
{
    public function new(): Response
    {
        // creates a task object and initializes some data for this example
        $task = new Task();
        $task->setTask('Write a blog post');
        $task->setDueDate(new \DateTime('tomorrow'));

        // build form
        $form = $this->createForm(TaskType::class, $task);

        // render
        return $this->renderForm('task/new.html.twig', [
            'form' => $form,
        ]);
    }
}
```

Optionally specify `data_class` option

```php
// src/Form/Type/TaskType.php
namespace App\Form\Type;

use App\Entity\Task;
use Symfony\Component\OptionsResolver\OptionsResolver;
// ...

class TaskType extends AbstractType
{
    // ...

    public function configureOptions(OptionsResolver $resolver): void
    {
        $resolver->setDefaults([
            'data_class' => Task::class,
        ]);
    }
}
```

Finally Render it in the Twig template

```twig
{# templates/task/new.html.twig #}
{{ form(form) }}
```

### Process Form Example

```php
// src/Controller/TaskController.php

// ...
use Symfony\Component\HttpFoundation\Request;

class TaskController extends AbstractController
{
    public function new(Request $request): Response
    {
        // just set up a fresh $task object (remove the example data)
        $task = new Task();

        $form = $this->createForm(TaskType::class, $task);

        $form->handleRequest($request);
        if ($form->isSubmitted() && $form->isValid()) {
            // $form->getData() holds the submitted values
            // but, the original `$task` variable has also been updated

            //HEY CHRIS TEST WHATS IN $TASK HERE LOL
            //dump($task);
            //return $this->redirectToRoute('task_success');

            $task = $form->getData();

            // ... perform some action, such as saving the task to the database

            return $this->redirectToRoute('task_success');
        }

        return $this->renderForm('task/new.html.twig', [
            'form' => $form,
        ]);
    }
}
```

## Styling | Form Theme

### Bootstrap 5

```yaml
# config/packages/twig.yaml
twig:
    form_themes: ['bootstrap_5_layout.html.twig']
```

### [Create Your Own Form Theme](https://symfony.com/doc/5.4/form/form_themes.html#create-your-own-form-theme)

## Debug Commands

```terminal
$ php bin/console debug:form

# pass the form type FQCN to only show the options for that type, its parents and extensions
# For built-in types, you can pass the short classname instead of the FQCN
$ php bin/console debug:form BirthdayType

# pass also an option name to only display the full destination of that option
$ php bin/console debug:form BirthdayType label_attr
```

FQCN = fully qualified name

## How To (Advanced)

### Change the Form Name

use `createNamed()` [method](https://github.com/symfony/symfony/blob/5.4/src/Symfony/Component/Form/FormFactoryInterface.php#:~:text=function%20createNamed)
```php
// src/Controller/TaskController.php
namespace App\Controller;

use App\Form\TaskType;
use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\Form\FormFactoryInterface;
// ...

class TaskController extends AbstractController
{
    public function new(FormFactoryInterface $formFactory): Response
    {
        $task = ...;
        $form = $formFactory->createNamed('my_name', TaskType::class, $task);

        // ...
    }
}
```

### Turn off Client-Side HTML Validation

```twig
{# templates/task/new.html.twig #}
{{ form_start(form, {'attr': {'novalidate': 'novalidate'}}) }}
    {{ form_widget(form) }}
{{ form_end(form) }}
```

### Form Type Guessing

To enable Symfony's "guessing mechanism", omit the second argument from the `add()` method or pass `null` to it

```php
class TaskType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            // if you don't define field options, you can omit the second argument
            ->add('task')
            // if you define field options, pass NULL as second argument
            ->add('dueDate', null, ['required' => false])
            ->add('save', SubmitType::class)
        ;
    }
}
```

#### Form Type Options Guessing

The `required` and `maxlength` field options will be guessed too 

- `required` checks if the
  - validation constraints `NotBlank` or `NotNull` is applied
  - Doctrine metadata (i.e. is the field nullable)
- `maxlength` checks if the
  - validation constraints `Length` or `Range` is used
  - Doctrine metadata (i.e. fields length)

Override:
`->add('task', null, ['attr' => ['maxlength' => 4]])`

### [How To Upload Files](https://symfony.com/doc/5.4/controller/upload_file.html)

### Unmapped Fields

When editing an object via form, all form fields are considered properties of the object.

Any fields on the form that do not tie to an object property will cause an exception to be thrown.

To fix set `mapped` option to `false` : (ex. for an "I agree with these terms" checkbox)
```php
// ...
use Symfony\Component\Form\Extension\Core\Type\CheckboxType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\FormBuilderInterface;

class TaskType extends AbstractType
{
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder
            ->add('task')
            ->add('dueDate')
            ->add('agreeTerms', CheckboxType::class, ['mapped' => false])
            ->add('save', SubmitType::class)
        ;
    }
}
```

Get and set `unmapped` fields in a controller
```php
$form->get('agreeTerms')->getData();
$form->get('agreeTerms')->setData(true);
```

Any fields on the form that arent submitted are set to null

### [How to Access Services or Config form Inside a Form](https://symfony.com/doc/5.4/form/form_dependencies.html)

### [Data Transformers](https://symfony.com/doc/5.4/form/data_transformers.html)

### [Data Mappers](https://symfony.com/doc/5.4/form/data_mappers.html)

### [Form Events](https://symfony.com/doc/5.4/form/events.html)

#### [How to Dynamically Modify Forms Using Form Events](https://symfony.com/doc/5.4/form/dynamic_form_modification.html)

### [Form Customization](https://symfony.com/doc/5.4/form/form_customization.html#reference-form-twig-functions)

### [Direct Submit](https://symfony.com/doc/5.4/form/direct_submit.html)

### [Form options constraints](https://symfony.com/doc/5.4/form/without_class.html#form-option-constraints)

### [Validation Groups](https://symfony.com/doc/5.4/form/validation_groups.html)

#### [Dynamically Configure Form Validation Groups](https://symfony.com/doc/5.4/form/validation_group_service_resolver.html)

#### [Choose Validation Groups Based on the Clicked Button](https://symfony.com/doc/5.4/form/button_based_validation.html)

#### [Disable the Validation of Submitted Data](https://symfony.com/doc/5.4/form/disabling_validation.html)