# Iniciar Sesion en Azure CLI

```
 Az Login
```

# Seleccionar la Suscripción sobre la que trabajaremos

```
 az account set --subscription tu-id-de-suscripcion 
```

# Obtener el id de todos los recursos dentro del grupo de recursos

```
 $resource = $(az resource list -g  nombre-grupo-de-recurso --query '[].id' --output tsv)
```

# Crea tus tags

estos tags generalmente son utilizados por entidades para llevar un control de sus recursos

```
 $gerencia = "test"
 $ceco = "cecotest"
 $ambiente = "dev-qa-prd"
 $iniciativa = " esto es una iniciativa"
```

# Asignar los tags a los recursos

```
 foreach($resource in $resource){
  az tag create --resource-id $resource --tags Gerencia=$gerencia Ceco=$ceco Ambiente=$ambiente Iniciativa=$iniciativa }
```

con esto puedes aplicar a recursos no tageados , de manera rapida y eficaz , los distintos tags que se declares relevantes para gestionar tus recursos en azure de una mejor manera.

cabe destacar que este scrip elimina tags , si es que el recurso posee ya tags con anterioridad, y aplica los declarados en el codigo.