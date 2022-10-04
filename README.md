
[<img src="https://arifpay.net/brand/ArifPay-Logo-(Full-Color).png" />](https://arifpay.net)

# Arifpay API Package.

[![Latest Version on Packagist](https://img.shields.io/packagist/v/arifpay/arifpay.svg?style=flat-square)](https://packagist.org/packages/arifpay/arifpay)
[![GitHub Tests Action Status](https://img.shields.io/github/workflow/status/arifpay/arifpay/run-tests?label=tests)](https://github.com/arifpay/arifpay/actions?query=workflow%3Arun-tests+branch%3Amain)
[![GitHub Code Style Action Status](https://img.shields.io/github/workflow/status/arifpay/arifpay/Check%20&%20fix%20styling?label=code%20style)](https://github.com/arifpay/arifpay/actions?query=workflow%3A"Check+%26+fix+styling"+branch%3Amain)
[![Total Downloads](https://img.shields.io/packagist/dt/arifpay/arifpay.svg?style=flat-square)](https://packagist.org/packages/arifpay/arifpay)

## Documentation

See the [`Developer` API docs](https://developer.arifpay.net/).


## Installation

You can install the package via composer:

```bash
composer require arifpay/arifpay
```

## Usage

The package needs to be configured with your account's API key, which is
available in the [Arifpay Dashboard](https://dashboard.arifpay.net/app/api). Require it with the key's
value. After install the package. you can use as follow.

 > :warning: Since V2 ``Arifpay->create()`` is deprecated and ``Arifpay->checkout->create()`` should be used.

```php
use Arifpay\Phpsdk\Arifpay;

...

$arifpay = new Arifpay('your-api-key');

```


## Creating Checkout Session

After importing the `arifpay` package, use the checkout property of the Arifpay instance to create or fetch `checkout sessions`.


```php
require_once 'vendor/autoload.php';


use Arifpay\Phpsdk\Arifpay;


use Arifpay\Phpsdk\Helper\ArifpaySupport;
use Arifpay\Phpsdk\Lib\ArifpayBeneficary;
use Arifpay\Phpsdk\Lib\ArifpayCheckoutItem;
use Arifpay\Phpsdk\Lib\ArifpayCheckoutRequest;
use Arifpay\Phpsdk\Lib\ArifpayOptions;

$arifpay = new Arifpay('your-api-key');

$expired = "2023-01-13T17:09:42.411";
$data = new ArifpayCheckoutRequest(
    cancel_url: 'https://api.arifpay.com',
    error_url: 'https://api.arifpay.com',
    notify_url: 'https://gateway.arifpay.net/test/callback',
    expireDate: $expired,
    nonce: floor(rand() * 10000) . "",
    beneficiaries: [
        ArifpayBeneficary::fromJson([
            "accountNumber" => '01320811436100',
            "bank" => 'AWINETAA',
            "amount" => 10.0,
        ]),
    ],
    paymentMethods: ["CARD"],
    success_url: 'https://gateway.arifpay.net',
    items: [
        ArifpayCheckoutItem::fromJson([
            "name" => 'Bannana',
            "price" => 10.0,
            "quantity" => 1,
        ]),
    ],
);
$session =  $arifpay->checkout->create($data, new ArifpayOptions(sandbox: true));
echo $session->session_id;

```


## Getting Session by Session ID

To track the progress of a checkout session you can use the fetch method as shown below:

```php
 $arifpay = new Arifpay('API KEY...');
// A sessionId will be returned when creating a session.
 $session = $arifpay->checkout->fetch('checkOutSessionID', new ArifpayOptions(true));
```

The following object represents a session

```php
{
  public int $id, 
  public ArifpayTransaction $transcation, 
  public float $totalAmount, 
  public bool $test,  
  public string $uuid, 
  public string $created_at, 
  public string $update_at
}
```

## Cancel Session by Session ID

If the merchant want to cancel a checkout session. it's now possible as shown below.

```php
 $arifpay = new Arifpay('API KEY...');
// A sessionId will be returned when creating a session.
 $session = $arifpay->checkout->cancel('checkOutSessionID', new ArifpayOptions(true));
```

The `ArifpayCheckoutSession` class is returned.

## DirectPay

learn more about [DirectPay here](https://developer.arifpay.net/docs/direcPay/overview)
### DirectPay for telebirr
```php 
     $session = $arifpay->checkout->create($data, new ArifpayOptions(true));

    return $arifpay->directPay->telebirr->pay($session->session_id);
```

### DirectPay for awash wallet
```php 
     $session = $arifpay->checkout->create($data, new ArifpayOptions(true));

    return $arifpay->directPay->awash_wallet->pay($session->session_id);
```

### DirectPay for awash
```php 
     $session = $arifpay->checkout->create($data, new ArifpayOptions(true));

    return $arifpay->directPay->awash->pay($session->session_id);
```

# Change Log

Released Date: `v1.1.1` Oct 03, 2022

- Initial Release

## More Information

- [DirectPay](https://developer.arifpay.net/docs/direcPay/overview)
- [Check Full Example](https://github.com/Arifpay-net/-sample)
- [REST API Version](https://developer.arifpay.net/docs/checkout/overview)
- [Mobile SDK](https://developer.arifpay.net/docs/clientSDK/overview)
- [Change Log](https://developer.arifpay.net/docs/nodejs/changelog)
- [Node JS](https://developer.arifpay.net/docs/nodejs/overview)
- [](https://developer.arifpay.net/docs//overview)
- [Change Log](https://developer.arifpay.net/docs//changelog)

## Credits

- [basliel](https://github.com/ba5liel)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
