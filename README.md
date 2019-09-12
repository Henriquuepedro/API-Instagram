# API Instagram PHP


## Introdução

A pouco tempo o instagram fez algumas alterações em sua API, sendo assim quebrando muitas aplicações e criando muitas cores de cabeça para os programador que utilizavam a API.

* Framework utilizado: Laravel 5.8
* PHP 7.1.3
* Composer 1.8.5

Para começar a utilizar a API é preciso se registrar como um [Desenvolvedor Instagram](https://www.instagram.com/developer/register/), após a se registrar será gerado `client_id` e `client_secret`

> Lembrando que o instagram se refere clientes como aplicativos. Portanto, client_id e client_secret são iguais.


## Instalação

> Como informado será utilizado o composer, recomendo utilizar o mesmo.

```
$ composer require cosenary/instagram
```


### Instalação

`use MetzWeb\Instagram\Instagram;`


`new Instagram(<array> / <string>);`

Utilize o `array` caso queira acessar dados do usuário autenticado.

```
$instagram = new Instagram(array(
	'apiKey'      => 'YOUR_APP_KEY',
	'apiSecret'   => 'YOUR_APP_SECRET',
	'apiCallback' => 'YOUR_APP_CALLBACK'
));
```

Utilize o `string` caso queira acessar apenas os dados publicos.

```
$instagram = new Instagram('YOUR_APP_KEY');
```

