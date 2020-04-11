```PHP
<?php
$bolo1 = [
    'açucar',
    'farinha de trigo',
    'ovo',
    'leite',
    'fermento em pó'
];
//com Spread
$bolo2 = [
    'vasilha',
    'agua morna',
    ...$bolo1,
    'corante'
];
echo $bolo2[2];//açucar

$lista1 = ['luiz', 'pedro', 'joão'];
$lista2 = ['patricia', 'ana', 'melissa'];
$lista3 = [...$lista1, ...$lista2, 'fulano'];
print_r($lista3);
//Array ( [0] => luiz [1] => pedro [2] => joão [3] => patricia [4] => ana [5] => melissa [6] => fulano )
```
