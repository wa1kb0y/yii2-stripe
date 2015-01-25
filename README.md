Yii2 Stripe Wrapper.
==========
<b>28/11/2014</b>
Simple and custom embedded <b>checkout</b> forms implemented. <br />
https://stripe.com/docs/checkout#integration-simple <br />
https://stripe.com/docs/checkout#integration-custom <br />

<b>09/12/2014</b>
Custom forms implemented and small fixes + composer. <br />
https://stripe.com/docs/tutorials/forms <br />

<b>TODO:</b>
Maybe want to add PayAction for a faster use. and Jquery Payment library as an optional add on. <br />
https://stripe.com/docs/tutorials/forms <br />
https://github.com/stripe/jquery.payment <br />

Installation
--------------------------

The preferred way to install this extension is through http://getcomposer.org/download/.

Either run

```sh
php composer.phar require ruskid/yii2-stripe "dev-master"
```

or add

```json
"ruskid/yii2-stripe": "dev-master"
```

to the require section of your `composer.json` file.


Usage
--------------------------
Add a new component in main.php
```php
'components' => [
...
'stripe' => [
    'class' => 'ruskid\stripe\Stripe',
    'publicKey' => "pk_test_xxxxxxxxxxxxxxxxxxx",
    'privateKey' => "sk_test_xxxxxxxxxxxxxxxxxx",
],
...

```

To render simple checkout form just call the widget in the view, it will automatically register the scripts.
Check stripe documentation for more options.
```php
use ruskid\stripe\StripeCheckout;
<?= 
StripeCheckout::widget([
    'action' => '/',
    'name' => 'Demo test',
    'description' => '2 widgets ($20.00)',
    'amount' => 2000,
    'image' => '/128x128.png',
]);
?>
```

Custom checkout form is an extended version of simple form, but you can customize the button (see buttonOptions) and handle token as you want (tokenFunction).
```php
<?= 
StripeCheckoutCustom::widget([
    'action' => '/',
    'name' => 'Demo test',
    'description' => '2 widgets ($20.00)',
    'amount' => 2000,
    'image' => '/128x128.png',
    'buttonOptions' => [
        'class' => 'btn btn-lg btn-success',
    ],
    'tokenFunction' => new JsExpression('function(token) { alert("Here you should control your token."); }'),
    'openedFunction' => new JsExpression('function() { alert("Model opened"); }'),
    'closedFunction' => new JsExpression('function() { alert("Model closed"); }'),
]);
?>
```
Full copy of https://stripe.com/docs/tutorials/forms.
Example of a custom form. StripeForm is an extended active form so you can perform validation of amount and other attributes you want. To render the card inputs use 4 following methods. You can change the token name that will be sent to your action and error's container id. You can also change JsExpression for response and request handlers.

```php
 <?php $form = StripeForm::begin([
        'tokenInputName' => 'stripeToken',
        'errorContainerId' => 'payment-errors',
 ]); ?>
 <?= $form->field($model, 'amount') ?>
 
 <?= $form->numberInput(['class' => 'form-control']) ?>
 <?= $form->cvcInput() ?>
 <?= $form->yearInput() ?>
 <?= $form->monthInput() ?>

 <div id="payment-errors"></div>
 <?php StripeForm::end(); ?>
```

