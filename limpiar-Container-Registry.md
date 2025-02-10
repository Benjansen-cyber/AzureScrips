## descripcion 

con este shell scrip , podremos limpiar de manera rapida container registry en azure , los cuales no han recibido un control y/o mantenimiento correspondiente durante el tiempo. ayuda con :

- eliminar exceso de imagenes sobre un repositorio
- eliminar imagenes con vulnerabilidad
- mantener con un numero de imagenes requerido el repositorio

## Iniciar Sesión en la CLI de Azure

```
 Az Login
```

## Seleccionar la cuenta o suscripcion sobre la cual trabajaremos 

```
 az account set --subscription tu-id-de-suscripcion 
```

## Declarar el container registry sobre el cual se trabajara

```
 $acr_name = ""
```
 
## obtener el nombre de los repositorios en el ACR

```
$repositorios = (az acr repository list --name $ACR_NAME --output tsv) 
```