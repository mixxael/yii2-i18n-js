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
            'class' => 'w3lifer\yii2\I18nJs',
        ],
        // ...
    ],
    // ...
];
```

---

**2. Initialize the `i18n` component anywhere, for example in `@app/views/layouts/main.php`:**

``` php
Yii::$app->i18nJs;
```

**Note**, you do not need to register the component in the places that will be processed with AJAX-requests (for example, in `@app/config/web.php` -> `bootstrap`, `on afterRequest`, etc), because it will be loaded twice, and it makes no sense.

---

**3. Create `@app/messages/ru/app.php` with the following content:**

``` php
<?php

return [
    'Hello, World!' => 'Привет, Мир!',
    'Hello, {username}!' => 'Привет, {username}!',
    'Hello, {0} {1}!' => 'Привет, {0} {1}!',
];
```

---

**4. Create `@app/controllers/I18nJsController`:**

``` php
<?php

namespace app\controllers;

use yii\web\Controller;

class I18nJsController extends Controller
{
    public function actionIndex(): string
    {
        return $this->render('index');
    }
}
```

---

**5. Create `@app/views/i-18-js/index.php`:**

``` php
<?php

/**
 * @var \yii\web\View $this
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

// Use case #2 (the contents of the [i18nTest.js] file are the same as in use case #3)

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

**6. Add `I18nJs::$jsFilename` (default: `@app/web/js/i18n.js`) to `@app/.gitignore`:**

``` gitignore
# w3lifer/yii2-i18n-js
/web/js/i18n.js
```

---

**7. Go to http://example.com/i-18n-js and view the result in your browser console.**
