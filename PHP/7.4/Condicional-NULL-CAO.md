# Null Coalescing Assignment Operator

```PHP
<?php
$nome = 'Luiz';
$nomeCompleto = $nome;

$nomeCompleto .= $sobrenome;  // Undefined variable: sobrenome
$nomeCompleto .= isset($sobrenome) ? \$sobrenome : ''; // ok
```

```PHP
<?php
$nomeCompleto = $nome ?? 'Visitante';
$nomeCompleto .= $sobrenome ?? '';

echo $nomeCompleto; // Visitante
```
