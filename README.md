# [yii2-i18n-js](https://packagist.org/packages/w3lifer/yii2-i18n-js)

- [Installation](#installation)
- [Usage](#usage)

## Installation

``` shell
composer require w3lifer/yii2-i18n-js
```

## Usage

**1. Configure the following components in `@app/config/web.php`:**

``` php
<?php

return [
    // ...
    'components' => [
        'i18n' => [ // https://www.yiiframework.com/doc/guide/2.0/en/tutorial-i18n
            'translations' => [
                '*' => [
                    'class' => \yii\i18n\PhpMessageSource::class,
                ],
            ],
        ],
        'i18nJs' => [
            'class' => \w3lifer\Yii2\I18nJs::class,
        ],
        // ...
    ],
    // ...
];
```

---

**2. Create `@app/messages/ru/app.php` with the following content:**

``` php
<?php

return [
    'Hello, World!' => 'Привет, Мир!',
    'Hello, {username}!' => 'Привет, {username}!',
    'Hello, {0} {1}!' => 'Привет, {0} {1}!',
];
```

---

**3. Create `@app/controllers/I18nJsController`:**

``` php
<?php

namespace app\controllers;

class I18nJsController extends \yii\web\Controller
{
    public function actionIndex(): string
    {
        return $this->render('index');
    }
}
```

---

**4. Create `@app/views/i-18-js/index.php` ...**

> **PLEASE NOTE** that the `i18n` component needs to be initialized before use.<br>
> You can do this anywhere, for example in `@app/views/layouts/main.php`, just touch it.<br>

``` php
Yii::$app->i18nJs;
```

> **BUT** don't touch the component in places that will be handled by AJAX requests<br>
> (e.g., in `@app/config/web.php` -> `bootstrap`, `on afterRequest`, etc),<br>
> because it will be loaded twice and that doesn't make sense.


``` php
<?php

/**
 * @see \app\controllers\I18nJsController::actionIndex()
 *
 * @var \yii\web\View $this
 *
 * @see http://localhost:809/i-18n-js
 */

Yii::$app->i18nJs;

Yii::$app->language = 'ru';

$this->title = 'I18nJs';

?>

<script>
    // Use case #1
    window.addEventListener('DOMContentLoaded', _ => {
      console.log('------------------- Use case #1')
      console.log(yii.t('app', 'Hello, World!')) // Привет, Мир!
      console.log(yii.t('app', 'Hello, {username}!', {username: 'John'})) // Привет, John!
      console.log(yii.t('app', 'Hello, {0} {1}!', ['John', 'Doe'])) // Привет, John Doe!
      console.log(yii.t('app', 'Hello, {0} {1}!', ['John', 'Doe'], 'en')) // Hello, John Doe!
    })
</script>

<?php

// Use case #2 (the contents of the [i18nTest.js] file are the same as in use case #1)

$this->registerJsFile('/js/i18nTest.js');

// Use case #3

$this->registerJs(<<<JS
    console.log('------------------- Use case #3')
    console.log(yii.t('app', 'Hello, World!')) // Привет, Мир!
    console.log(yii.t('app', 'Hello, {username}!', {username: 'John'})) // Привет, John!
    console.log(yii.t('app', 'Hello, {0} {1}!', ['John', 'Doe'])) // Привет, John Doe!
    console.log(yii.t('app', 'Hello, {0} {1}!', ['John', 'Doe'], 'en')) // Hello, John Doe!
JS);
```

---

**5. Add `I18nJs::$jsFilename` (default: `@app/web/js/i18n.js`) to `@app/.gitignore`:**

``` gitignore
# w3lifer/yii2-i18n-js
/web/js/i18n.js
```

---

**6. Go to http://example.com/i-18n-js and view the result in your browser console.**
