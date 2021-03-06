# Esempi

Sotto gli esempi proposti durante la parte pratica

<!-- TOC -->

- [Esempi](#esempi)
  - [Tematizzare usando le espressioni](#tematizzare-usando-le-espressioni)
    - [caso semplice](#caso-semplice)
  - [Etichettare usando le espressioni](#etichettare-usando-le-espressioni)
    - [caso semplice](#caso-semplice-1)
  - [Tabella degli attributi](#tabella-degli-attributi)
    - [vari casi](#vari-casi)
  - [Selezione per espressione](#selezione-per-espressione)
    - [attributi alfanumerici](#attributi-alfanumerici)
    - [geometria](#geometria)
  - [Statistiche](#statistiche)
  - [Widget](#widget)
  - [Gruppo Funzioni Array](#gruppo-funzioni-array)
  - [Gruppo Funzioni Maps](#gruppo-funzioni-maps)
  - [Compositore di stampe e grafico a colori](#compositore-di-stampe-e-grafico-a-colori)
  - [Affine Transform](#affine-transform)
- [RIFERIMENTI](#riferimenti)

<!-- /TOC -->

## Tematizzare usando le espressioni

### caso semplice

espressione

```
substr( "den_reg",-1,1)
```

![](./imgs/esempi/img_01.png)

↑ [TORNA SU](#esempi) ↑

## Etichettare usando le espressioni

### caso semplice

```
centroid(
collect($geometry, "cod_reg" ))
```

![](./imgs/esempi/img_02.png)

↑ [TORNA SU](#esempi) ↑

## Tabella degli attributi

### vari casi

```
UPPER("COD_REG")
```

↑ [TORNA SU](#esempi) ↑

## Selezione per espressione

### attributi alfanumerici

```
"den_uts"
ilike
'%z%'
```

![](./imgs/esempi/img_03.png)

↑ [TORNA SU](#esempi) ↑

### geometria

```
 maximum(
 expression:=$area,
 group_by:="cod_reg")
 =
 $area
 ```

![](./imgs/esempi/img_04.png)

↑ [TORNA SU](#esempi) ↑

## Statistiche

![](./imgs/esempi/img_05.png)

## Widget

![](./imgs/esempi/img_06.png)

[blog post](https://pigrecoinfinito.com/2021/07/15/dati-sulla-vaccinazione-in-sicilia/)

```
import QtQuick 2.0
import QtCharts 2.0

ChartView {
    title: "Bar series"
    width: 800
    height:400
    legend.alignment: Qt.AlignBottom
    antialiasing: true
    ValueAxis{
        id: valueAxisY
        min: 0
        max: expression.evaluate("array_max(
map_avals(
map_delete(map_delete(map_delete(
attributes(),'fid'),'PRO_COM'),'COMUNE')))*1.10")
    }

    BarSeries {
        id: mySeries
        axisY: valueAxisY
        axisX: BarCategoryAxis { categories: expression.evaluate("map_akeys(
map_delete(map_delete(map_delete(
attributes(),'fid'),'PRO_COM'),'COMUNE'))") }
        BarSet { label: "Anno"; values: expression.evaluate("map_avals(
map_delete(map_delete(map_delete(
attributes(),'fid'),'PRO_COM'),'COMUNE'))")}
    }
}
```

↑ [TORNA SU](#esempi) ↑

## Gruppo Funzioni Array

Selezionare i primi 10 punti più vicini al punto selezionato

![](./imgs/esempi/img_07.png)

[54esimo esempio #HfcQGIS](https://hfcqgis.opendatasicilia.it/esempi/selezionare_punti_vicini_punto/)

↑ [TORNA SU](#esempi) ↑

## Gruppo Funzioni Maps

Determinare il nome del campo che contiene il valore più grande

```
with_variable('deleteField',
        map_delete(map_delete(map_delete(
        attributes(),'fid'),'PRO_COM'),'COMUNE'),
with_variable('sel',
        map_avals( @deleteField),
        map_akeys( @deleteField)[array_find(@sel,array_max(@sel))])
             )
```

![](./imgs/esempi/img_08.png)

[55esimo esempio #HfcQGIS](https://hfcqgis.opendatasicilia.it/esempi/maxValoreCampoNome/)

↑ [TORNA SU](#esempi) ↑

## Compositore di stampe e grafico a colori

funzione personalizzata

[get_catg_color](https://hfcqgis.opendatasicilia.it/gr_funzioni/custom/custom_unico/#get_catg_color)

![](./imgs/esempi/img_09.png)

↑ [TORNA SU](#esempi) ↑

## Affine Transform

![](./imgs/esempi/img_10.png)

```
with_variable('bbox', bounds($geometry),
with_variable('x_min', x(centroid(@bbox)),
with_variable('y_min', y(centroid(@bbox)),
	translate(
		affine_transform( 
			geometry:= translate( $geometry, -@x_min,-@y_min),
			deltaX:=0,
			deltaY:=0,
			rotationZ:=45,
			scaleX:=1,
			scaleY:=1),
	@x_min,@y_min))))
```

↑ [TORNA SU](#esempi) ↑

# RIFERIMENTI

- [HfcQGIS](https://hfcqgis.opendatasicilia.it/)
- [QGIS](https://www.qgis.org/it/site/)