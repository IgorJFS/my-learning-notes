# Web Assets Quick Reference
Lista de CDNs úteis e exemplos de como incluir CSS e JavaScript no HTML. Ideal para copiar e colar rapidamente em projetos web.
## Como Incluir CSS e JS no HTML
### CSS (no <head>)
```html
<head>
  <link href="URL_DO_CDN" rel="stylesheet">
</head>
```
### JS (antes do </body>)
```html
<body>
  <!-- Conteúdo -->
  <script src="URL_DO_CDN"></script>
</body>
```
## CDNs - CSS
**Bootstrap 5.3.3**:
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">

## CDNs - JavaScript
**jQuery 3.7.1**:
<script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>

**Bootstrap 5.3.3** (JS, inclui Popper.js):
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>

**Axios 1.7.2**:
<script src="https://cdn.jsdelivr.net/npm/axios@1.7.2/dist/axios.min.js" integrity="sha256-MfXcwF9U5mMSEb0S2PfsLCOXOLPW6CSzUhjTGpFjvgM=" crossorigin="anonymous"></script>

## Exemplo Completo
```html
<!DOCTYPE html>
<html>
<head>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
</head>
<body>
  <h1>Hello World! :D</h1>
  <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
</body>
</html>
```
