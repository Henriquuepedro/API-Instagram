# API Instagram PHP


## Introdução
> A pouco tempo o instagram fez algumas alterações em sua API, sendo assim quebrando muitas aplicações e criando muitas cores de cabeça para os programador que utilizavam a API.

> Com essa API é possível recuperar e usar diversas informações de sua conta ou até mesmo de conta de outros usuários, sendo dividido em métodos publicos e métodos do usuário autenticado.

> O retorno dessa API é feita por JSON.

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
## Instalação e Configuração

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

## Métodos

### Obter URL de login
`getLoginUrl(<array>)`
``` 
getLoginUrl(array(
	'basic',
	'likes'
));
```

Escopo 	  	| Legenda 					| Metodos
--------- 	| ------ 					| ------
`basic`     	| to use all user related methods [default] 	| `getUser()`, `getUserFeed()`, `getUserFollower()` etc.
`comments`    	| to create or delete comments			| `getMediaComments()`, `addMediaComment()`, `deleteMediaComment()`
`likes`		| to like and unlike items			| `getMediaLikes()`, `likeMedia()`, `deleteLikedMedia()`
`relationships`	| to follow and unfollow users			| `modifyRelationship()`

### Obter token [OAuth](https://tools.ietf.org/html/draft-ietf-oauth-v2-12)

`getOAuthToken($code, <true>/<false>)`
`true` : Returns only the OAuth token
`false` *[default]* : Returns OAuth token and profile data of the authenticated user

### Set/Get token de acesso

* `setAccessToken($token)` assina a variável para ser usada em outros métodos.
* `getAccessToken()` Obtem o token de acesso, caso queira guarda-lo.


### Métodos do usuário autenticado

#### Métodos públicos
* `getUser($id)`
* `searchUser($name, <$limit>)`
* `getUserMedia($id, <$limit>)`

### Métodos autenticados

* `getUser()`
* `getUserLikes(<$limit>)`
* `getUserFeed(<$limit>)`
* `getUserMedia(<$id>, <$limit>)`
	* se um `$id` não for passado ou for igual a 'self', ele retornará a mídia do usuário autenticado

> [Pontos de extremidade do usuário](https://www.instagram.com/developer/endpoints/users)

### Métodos de relacionamento

#### Métodos autenticados

* `getUserFollows($id, <$limit>)`
* `getUserFollower($id, <$limit>)`
* `getUserRelationship($id)`
* `modifyRelationship($action, $user)`
	* `$action` : Comando de ação (follow / unfollow / block / unblock / approve / deny)
	* `$user` : ID do usuário para consulta

```
Siga o usuário com o ID 13320420231
$instagram->modifyRelationship('follow', 13320420231);
```
---------------------------------------
Observe que o método `modifyRelationship()` requer o relationships [escopo](https://github.com/Henriquuepedro/API-Instagram#get-login-url).

### Métodos de mídias

#### Métodos públicos

* `getMedia($id)`
	* `authenticated users receive the info, whether the queried media is liked
* `getPopularMedia()`
* `searchMedia($lat, $lng, <$distance>, <$minTimestamp>, <$maxTimestamp>)`
	* `$lat` e `$lng` são coordenadas e passados como `float`: `-27.5969`, `-48.5495`
	* `$distance` : Distância radial em metros (padrão: é 1km = 1000, max. é 5km = 5000)
	* `$minTimestamp` : Todas as mídias retornadas serão obtidas depois desta data/hora (padrão: 5 dias atrás)
	* `$maxTimestamp` : Todas as mídias retornadas serão obtidas antes desta data/hora (padrão: Agora)


### Métodos de comentário

#### Métodos públicos

* `getMediaComments($id)`

#### Métodos autenticados

* `addMediaComment($id, $text)`
	* **acesso restrito:** envie um email `apidevelopers@instagram.com` para ter acesso
* `deleteMediaComment($id, $commentID)`
	* Comentário deve ser de autoria do usuário autenticado

---------------------------------------
Observe que os métodos autenticados requerem o `comments` [escopo](https://github.com/Henriquuepedro/API-Instagram#get-login-url).
---------------------------------------
> [Exemplos de respostas dos terminais de comentários.](https://www.instagram.com/developer/endpoints/comments)

### Métodos de tag

#### Métodos públicos

* `getTag($name)`
* `getTagMedia($name)`
* `searchTags($name)`



### Métodos de curtidas

#### Métodos autenticados

* `getMediaLikes($id)`
* `likeMedia($id)`
* `deleteLikedMedia($id)`

> Todos os `<...>` parâmetros são opcionais. Se não for passado nenhum, todos os resultados disponíveis serão retornados.


## Exemplo de uso

```
<?php
require 'Instagram.php';
use MetzWeb\Instagram\Instagram;
$instagram = new Instagram(array(
  'apiKey'      => 'YOUR_APP_KEY',
  'apiSecret'   => 'YOUR_APP_SECRET',
  'apiCallback' => 'YOUR_APP_CALLBACK'
));
$token = 'USER_ACCESS_TOKEN';
$instagram->setAccessToken($token);
$id = 'MEDIA_ID';
$result = $instagram->likeMedia($id);
if ($result->meta->code === 200) {
  echo 'Success! The image was added to your likes.';
} else {
  echo 'Something went wrong :(';
}
```

## Referência

[Cosenary](https://github.com/cosenary/Instagram-PHP-API)



