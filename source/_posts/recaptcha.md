---
title: recaptcha
date: 2019-01-14 23:31:59
tags:
---
# recaptcha v3

## Apply

Apply on [Admin console](https://g.co/recaptcha/v3)

![alt text](https://i.imgur.com/oUXeDZM.png)

Choose v3，filled up domains which need to be validated.

Example：
 - 127.0.0.1

## Code

### blade or HTML

```htmlembedded=
<form>
    <input type="text" id="g-recaptcha-response" name="g-recaptcha-response" style="display: none;">
    <input type="name" id="name">
    <input type="submit">
</form>
```

### blade or JS
```htmlembedded=
<script src='https://www.google.com/recaptcha/api.js?render=SITE_KEY'></script>
<script>
  grecaptcha.ready(function() {
  grecaptcha.execute('SITE_KEY', {action: 'your_action'})
    .then(function(token){
      document.getElementById("g-recaptcha-response").value = token
    })
  });
</script>

```

### php
```php=
public function recaptchaCheck($recaptchaToken, $action)
{
    $secret_key = config('app.NOCAPTCHA_SECRET');
    $client = new Client();
    $result = $client->post('https://www.google.com/recaptcha/api/siteverify', [
        'form_params' => [
            'secret' => $secret_key,
            'response' => $recaptchaToken,
        ],
    ]);
    $result = json_decode($result->getBody(), true);
    return (isset($result['success']) && $result['success']) &&
        (isset($result['score']) && $result['score'] > 0.5) &&
        (isset($result['action']) && $result['action'] === $action);
}

```

## Reference
 - https://www.google.com/recaptcha/intro/v3.html
 - https://www.youtube.com/watch?v=w6WF9rw6-3c