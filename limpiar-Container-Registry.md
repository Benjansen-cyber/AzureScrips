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

debes ingresar a la suscripcion en donde se encuentra el ACR que modificaremos.

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
$imagenes_a_guardar = 4
foreach ($repo in $repositorios) {
	$REPO_NAME = $repo
	$tags = (az acr repository show-tags --name $ACR_NAME --repository $REPO_NAME --orderby time_desc --output tsv)
	$imagenes_guardar=@()
	$imagenes_borrar=@()
	$contador=0
    foreach ($tag in $tags) {
		if ($contador -lt 4){
			$imagenes_guardar += $tag
		} else {
			$imagenes_borrar += $tag
			az acr repository delete --name $ACR_NAME --image {$REPO_NAME}:{$tag}
		}
		$contador++
    }
	Write-Output "Imagenes Guardadas : $imagenes_guardar"
	Write-Output "Imagenes Eliminadas : $imagenes_borrar"
}
```
1. primero en nuestra variable $imagenes_a_guardar declararemos la cantidad de imagenes que deseamos mantener en el repositorio.
2. Luego obtendremos los identificadores de las imagenes ( tags ) ordenando estas por fecha. desde la mas nueva a la mas antigua. en este ejemplo luego de guardar las 4 ultimas imagenes , las siguientes seran eliminadas.

3. se mostrara en la consola las imagenes que han sido guardadas y las eliminadas.

## 🚧⚡

Utiliza esta ayuda con precaución , debido a que al ejecutarlo no tendras una manera de recuperar las imagenes que elimines del ACR.