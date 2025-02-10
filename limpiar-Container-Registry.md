## Descripcion 

con este shell scrip , podremos limpiar de manera rapida container registry en azure , los cuales no han recibido un control y/o mantenimiento correspondiente durante el tiempo. ayuda con :

- eliminar exceso de imagenes sobre un repositorio 🚮
- eliminar imagenes con vulnerabilidad 🚮
- mantener con un numero de imagenes requerido el repositorio 🛂

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

## recorrer repositorios para identificar cuantas imagenes se deberean eliminar

```
foreach ($repo in $repositorios) {
    Write-Output "Actuando sobre Repositorio: $repo"
	$REPO_NAME = $repo
	$tags = (az acr repository show-tags --name $ACR_NAME --repository $REPO_NAME --orderby time_desc --output tsv)
	$contador=0
	$imagenes_guardar=@()
	$imagenes_borrar=@()
    foreach ($tag in $tags) {
        Write-Output "Trabajando en imagen: $tag"
		if ($contador -lt 2){
			$imagenes_guardar += $tag
		} else {
			$imagenes_borrar += $tag
		}
		$contador++
    }
	Write-Output "Imagenes Guardadas : $imagenes_guardar"
	Write-Output "Imagenes Eliminadas : $imagenes_borrar"
}
```