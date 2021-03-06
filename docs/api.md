# API

Actualmente la API está disponible en `https://api.scltrans.it`

## Stops (`Paraderos`)

Información sobre paraderos o estaciones.

### Listar paraderos

> **Endpoint**

```curl
/v1/stops
```

> **Query params**

| property  | opcional       | default         | description                                         |
| --------- | :-------:  | :-------------: | --------------------------------------------------- |
| `limit`  | `si`   | `100`       | Cantidad de resultados por página.               |
| `page`     | `si`   | `1` | Número de página |
| `agency_id`     | `si`   | `None` | El campo agency_id es un ID que identifica de forma exclusiva a una empresa de transporte público. |
| `is_active`  | `si`   | `None`       | Opción para filtrar paraderos en funcionamiento. Ejemplo: is_active=1  |
| `center_lon` | `si` | `None`      | Longitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lat`. Ejemplo: `-70.643562`      |
| `center_lat` | `si` | `None`      | Latitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lon`. Ejemplo: `-33.491585`      |
| `radius` | `si` | `None`      | Radio en metros. Usar en conjunto con `center_lat` y `center_lon`. Si es definido, se mostrarán sólo los resultados dentro de ese radio (en relación al centro (center_lat` y `center_lon`))     |
| `bbox` | `si` | `None`      |  Bounding box (min Longitude , min Latitude , max Longitude , max Latitude). Si es definido, se mostrarán sólo los resultados dentro de este bbox.  Ejemplo: `-70.609818,-33.442328,-70.566473,-33.409806`    |


!> **NOTA**  `center_lon`, `center_lat` y `radius` formar parte del mismo filtro de geolocalización. No se debe usar en conjunto con `bbox` (son 2 filtros independientes). 

> **Respuesta**

Lista de [Stops](#Stop)


> **Ejemplo**

- Consulta

```curl
/v1/stops?limit=3
```

- Respuesta


```json
{
    "has_next": true,
    "page_number": 1,
    "total_results": 5231,
    "total_pages": 5231,
    "results": [
        {
            "stop_lat": "-33.426908855",
            "stop_code": "PC2",
            "stop_lon": "-70.615901969",
            "agency_id": "TS",
            "stop_id": "PC2",
            "directions": [
                {
                    "direction_id": 0,
                    "direction_headsign": "Mall P. Tobalaba",
                    "direction_name": "Outbound"
                }
            ],
            "stop_name": "PC2-Parada 7 / (M) Pedro   De Valdivia"
        },
        {
            "stop_lat": "-33.455996455",
            "stop_code": "PI438",
            "stop_lon": "-70.699194124",
            "agency_id": "TS",
            "stop_id": "PI438",
            "directions": [
                {
                    "direction_id": 0,
                    "direction_headsign": "La Florida",
                    "direction_name": "Outbound"
                }
            ],
            "stop_name": "PI438-Parada 1 / (M) Ecuador"
        },
        {
            "stop_lat": "-33.463569085",
            "stop_code": "PI396",
            "stop_lon": "-70.722582982",
            "agency_id": "TS",
            "stop_id": "PI396",
            "directions": [
                {
                    "direction_id": 0,
                    "direction_headsign": "La Florida",
                    "direction_name": "Outbound"
                }
            ],
            "stop_name": "PI396-Parada  / Paradero 5 1/2   Pajaritos"
        }
    ],
    "page_size": 3
}
```

### Obtener paradero

> **Endpoint**

```curl
/v1/stops/<stop_id>
```

> **Argumentos**

  - `{string} stop_id`: identificador de paradero


> **Respuesta**

[Objeto de Stop](#Stop)

> **Ejemplo**

- Consulta

```curl
/v1/stops/PB1
```

- Respuesta

```json
{
    "stop_lat": "-33.404553756",
    "stop_code": "PB1",
    "stop_lon": "-70.623095148",
    "agency_id": "TS",
    "stop_id": "PB1",
    "directions": [
        {
            "direction_id": 0,
            "direction_headsign": "Cerrillos",
            "direction_name": "Outbound"
        }
    ],
    "stop_name": "PB1-Venezuela Esq. / Bolivia"
}
```

### Estimación de próximos arribos

Información sobre próximos arribos en paraderos. Esta información es obtenida en tiempo real utilizando el Web Service de predicción provisto por la dirección de transporte público metropolitano.

!> **NOTA**: Lamentablemente, el webservice oficial (SMSBUS) que es utilizado para las predicciones no es del todo estable. Debes manejar posibles timeouts de la respuesta (respuestas con status code `408`) y excepciones no controladas (errores 503). Se está gestionando una solución con la empresa que está a cargo del servicio.

?> **OJO**: Este endpoint actualmente está en `v2`. 

> **Endpoint**

```curl
/v2/stops/<stop_id>/next_arrivals
```
> **Argumentos**

  - `{string} stop_id`: identificador de paradero

> **Respuesta**

Lista de [arribos](#Arrival)

> **Ejemplo**

- Consulta

```curl
/v2/stops/PH84/next_arrivals
```

- Respuesta

```json
{
    "results": [
        {
            "bus_plate_number": "ZN-3992",
            "arrival_estimation": "Menos de 5 min.",
            "calculated_at": "2017-11-24 13:31",
            "route_id": "125",
            "is_live": true,
            "direction_id": 1,
            "bus_distance": "591"
        },
        {
            "bus_plate_number": "ZN-6736",
            "arrival_estimation": "Entre 14 Y 22 min. ",
            "calculated_at": "2017-11-24 13:31",
            "route_id": "125",
            "is_live": true,
            "direction_id": 0,
            "bus_distance": "4141"
        },
        {
            "bus_plate_number": "FLXP-63",
            "arrival_estimation": "Entre 09 Y 15 min. ",
            "calculated_at": "2017-11-24 13:31",
            "route_id": "H03",
            "is_live": true,
            "direction_id": 1,
            "bus_distance": "3151"
        },
        {
            "bus_plate_number": null,
            "arrival_estimation": "No hay buses que se dirijan al paradero",
            "calculated_at": "2017-11-24 13:31",
            "route_id": "345",
            "is_live": true,
            "direction_id": 1,
            "bus_distance": null
        },
        {
            "bus_plate_number": null,
            "arrival_estimation": "Servicio fuera de horario de operacion para ese paradero",
            "calculated_at": "2017-11-24 13:31",
            "route_id": "346N",
            "is_live": true,
            "direction_id": 1,
            "bus_distance": null
        }
    ]
}
```


### Listar recorridos de paradero

Lista de recorridos, con su respectiva dirección, para un paradero específico. 


> **Endpoint**

```curl
/v3/stops/<stop_id>/stop_routes
```

?> **OJO**: Este endpoint actualmente está en `v3`. 


> **Argumentos**

  - `{string} stop_id`: identificador de paradero

> **Respuesta**

Lista de [StopRoutes](#StopRoute)

> **Ejemplo**

- Consulta

```curl
/v3/stops/PB8/stop_routes
```

- Respuesta

```json
{
    "results": [
        {
            "direction": {
                "direction_id": 0,
                "route_id": "107c",
                "direction_headsign": "Plaza Renca",
                "direction_name": "Outbound"
            },
            "is_first_stop": false,
            "is_last_stop": false,
            "route": {
                "route_long_name": "Ciudad Empresarial - Plaza Renca",
                "route_type": "3",
                "route_text_color": "000000",
                "agency_id": "TS",
                "route_id": "107c",
                "route_color": "00D5FF",
                "route_desc": null,
                "directions": [
                    {
                        "direction_id": 1,
                        "route_id": "107c",
                        "direction_headsign": "C. Empresarial",
                        "direction_name": "Inbound"
                    },
                    {
                        "direction_id": 0,
                        "route_id": "107c",
                        "direction_headsign": "Plaza Renca",
                        "direction_name": "Outbound"
                    }
                ],
                "route_url": null,
                "route_short_name": "107c"
            }
        },
        {
            "direction": {
                "direction_id": 1,
                "route_id": "107",
                "direction_headsign": "C. Empresarial",
                "direction_name": "Inbound"
            },
            "is_first_stop": false,
            "is_last_stop": false,
            "route": {
                "route_long_name": "Ciudad Empresarial - Av. Departamental",
                "route_type": "3",
                "route_text_color": "000000",
                "agency_id": "TS",
                "route_id": "107",
                "route_color": "00D5FF",
                "route_desc": null,
                "directions": [
                    {
                        "direction_id": 0,
                        "route_id": "107",
                        "direction_headsign": "Av. Departamental",
                        "direction_name": "Outbound"
                    },
                    {
                        "direction_id": 1,
                        "route_id": "107",
                        "direction_headsign": "C. Empresarial",
                        "direction_name": "Inbound"
                    }
                ],
                "route_url": null,
                "route_short_name": "107"
            }
        }
    ]
}
```

## Routes (`Recorridos`)

Recorridos de transporte público. Un recorrido o ruta es un grupo de viajes que se muestran a los usuarios como servicio independiente.

### Listar recorridos

> **Endpoint**

```curl
/v1/routes
```

> **Query params**

| property  | opcional       | default         | description                                         |
| --------- | :-------:  | :-------------: | --------------------------------------------------- |
| `limit`  | `si`   | `100`       | Cantidad de resultados por página.               |
| `page`     | `si`   | `1` | Número de página |
| `agency_id`     | `si`   | `None` | El campo agency_id es un ID que identifica de forma exclusiva a una empresa de transporte público. |


> **Respuesta**

Lista de [Routes](#Route)


> **Ejemplo**

- Consulta

```curl
/v1/routes?limit=3
```

- Respuesta

```json
{
    "has_next": true,
    "page_number": 1,
    "total_results": 254,
    "total_pages": 254,
    "results": [
        {
            "route_long_name": "(M) Blanqueado - Mall Plaza Tobalaba",
            "end_date": "2018-03-31",
            "route_type": "3",
            "route_text_color": "000000",
            "agency_id": "TS",
            "route_id": "102",
            "route_color": "00D5FF",
            "route_desc": null,
            "directions": [
                {
                    "direction_id": 0,
                    "route_id": "102",
                    "direction_headsign": "Mall P. Tobalaba",
                    "direction_name": "Outbound"
                },
                {
                    "direction_id": 1,
                    "route_id": "102",
                    "direction_headsign": "(M) Blanqueado",
                    "direction_name": "Inbound"
                }
            ],
            "route_url": null,
            "route_short_name": "102",
            "start_date": "2017-11-07"
        },
        {
            "route_long_name": "Recoleta - Cerrillos",
            "end_date": "2018-03-31",
            "route_type": "3",
            "route_text_color": "000000",
            "agency_id": "TS",
            "route_id": "101",
            "route_color": "00D5FF",
            "route_desc": null,
            "directions": [
                {
                    "direction_id": 0,
                    "route_id": "101",
                    "direction_headsign": "Cerrillos",
                    "direction_name": "Outbound"
                },
                {
                    "direction_id": 1,
                    "route_id": "101",
                    "direction_headsign": "Recoleta",
                    "direction_name": "Inbound"
                }
            ],
            "route_url": null,
            "route_short_name": "101",
            "start_date": "2017-11-07"
        },
        {
            "route_long_name": "(M) Blanqueado - Cerrillos",
            "end_date": "2018-03-30",
            "route_type": "3",
            "route_text_color": "000000",
            "agency_id": "TS",
            "route_id": "101c",
            "route_color": "00D5FF",
            "route_desc": null,
            "directions": [
                {
                    "direction_id": 0,
                    "route_id": "101c",
                    "direction_headsign": "Cerrillos",
                    "direction_name": "Outbound"
                },
                {
                    "direction_id": 1,
                    "route_id": "101c",
                    "direction_headsign": "(M) Blanqueado",
                    "direction_name": "Inbound"
                }
            ],
            "route_url": null,
            "route_short_name": "101c",
            "start_date": "2017-11-07"
        }
    ],
    "page_size": 3
}
```


### Obtener recorrido

> **Endpoint**

```curl
/v1/routes/<route_id>
```

> **Argumentos**

  - `{string} route_id`: identificador de ruta

> **Respuesta**

[Objeto de Route](#Route)

> **Ejemplo**

- Consulta

```curl
/v1/routes/102
```

- Respuesta

```json
{
    "route_long_name": "(M) Blanqueado - Mall Plaza Tobalaba",
    "end_date": "2018-03-31",
    "route_type": "3",
    "route_text_color": "000000",
    "agency_id": "TS",
    "route_id": "102",
    "route_color": "00D5FF",
    "route_desc": null,
    "directions": [
        {
            "direction_id": 0,
            "route_id": "102",
            "direction_headsign": "Mall P. Tobalaba",
            "direction_name": "Outbound"
        },
        {
            "direction_id": 1,
            "route_id": "102",
            "direction_headsign": "(M) Blanqueado",
            "direction_name": "Inbound"
        }
    ],
    "route_url": null,
    "route_short_name": "102",
    "start_date": "2017-11-07"
}
```

### Direcciones - detalles recorrido

Se muestra la información detallada de cada una de las direcciones para un recorrido (ruta). Este endpoint expone a su vez información del viaje activo para cada una de las direcciones.

> **Endpoint**

?> **NOTA**: A pesar de que el `v1` de este endpoint sigue existiendo por propósitos de retro-compatibilidad, es altamente recomendable utilizar `v2`. 

?> **TEN EN CUENTA**: Dado que es necesario realizar una serie de cálculos complejos para determinar la información a devolver, este endpoint tiene un tiempo de respuesta alto, entre 1 y 6 segundos.

```curl
/v2/routes/<route_id>/directions
```

> **Argumentos**

  - `{string} route_id`: identificador de ruta

> **Respuesta**

Lista de [direcciones](#Direction) con información detallada

> **Ejemplo**

- Consulta

/v2/routes/102/directions

```json
{
    "results": [
        {
            "direction_name": "Outbound",
            "route_id": "102",
            "stop_times": [
                {
                    "stop": "PI1330",
                    "arrival_time": "00:00:00",
                    "stop_sequence": 1,
                    "trip_id": "102-I-L_V36-B06",
                    "departure_time": "00:00:00"
                },
                {
                    "stop_id": "PI1201",
                    "arrival_time": "00:01:18",
                    "stop_sequence": 2,
                    "trip_id": "102-I-L_V36-B06",
                    "departure_time": "00:01:18"
                }
            ],
            "direction_id": 0,
            "shape": [
                {
                    "shape_pt_lat": "-33.463178001",
                    "shape_id": "102I_V36",
                    "shape_pt_lon": "-70.695377000",
                    "shape_pt_sequence": 1
                },
                {
                    "shape_pt_lat": "-33.463091001",
                    "shape_id": "102I_V36",
                    "shape_pt_lon": "-70.695304000",
                    "shape_pt_sequence": 2
                }
            ],
            "direction_headsign": "Mall P. Tobalaba"
        },
        {
            "direction_name": "Inbound",
            "route_id": "102",
            "stop_times": [
                {
                    "stop_id": "PF5",,
                    "arrival_time": "00:00:00",
                    "stop_sequence": 1,
                    "trip_id": "102-R-L_V36-B06",
                    "departure_time": "00:00:00"
                }
            ],
            "direction_id": 1,
            "shape": [
                {
                    "shape_pt_lat": "-33.579407001",
                    "shape_id": "102R_V36",
                    "shape_pt_lon": "-70.551336000",
                    "shape_pt_sequence": 1
                },
                {
                    "shape_pt_lat": "-33.579116001",
                    "shape_id": "102R_V36",
                    "shape_pt_lon": "-70.551586000",
                    "shape_pt_sequence": 2
                }
            ],
            "direction_headsign": "(M) Blanqueado"
        }
    ]
}
```

### Información dirección

Se muestra la información detallada de una dirección para una ruta.

> **Endpoint**

```curl
/v2/routes/<route_id>/directions/<direction_id>
```

> **Argumentos**

  - `{string} route_id`: identificador de ruta
  - `{string} direction_id`: identificador de dirección

> **Respuesta**

[Dirección con información detallada](#Direction)

> **Ejemplo**

- Consulta

/v2/routes/102/directions/0

```json
{
    "results": {
        "direction_name": "Outbound",
        "route_id": "102",
        "stop_times": [
            {
                "stop_id": "PI1330",
                "arrival_time": "00:00:00",
                "stop_sequence": 1,
                "trip_id": "102-I-L_V36-B06",
                "departure_time": "00:00:00"
            },
                "stop_id": "PI1201",
                "arrival_time": "00:01:18",
                "stop_sequence": 2,
                "trip_id": "102-I-L_V36-B06",
                "departure_time": "00:01:18"
            }
        }
}
```

## Map

Colección de elementos geolocalizados. Este endpoint es un "helper" que reúne diferentes elementos en un único método para facilitar la consulta. Actualmente los elementos disponibles son:

* [Stops](#Stop)
* [Puntos BIP](#BIP-Spot)
* [Buses](#Bus) 

?> **NOTA** Por defecto este endpoint sólo lista las [stops](#Stop), el resto son elementos opcionales configurables.

?> **OJO**: Este endpoint actualmente está en `v2`. 

### Listar elementos

> **Endpoint**

```curl
/v2/map
```

> **Query params**

| property  | opcional       | ejemplo  | description                                         |
| --------- | :-------:  | :-------------: | -------- ---------------------------------------------------- |
| `include_buses`     | `si`   | 1 | Incluir [buses](#Bus)  |
| `include_bip_spots`     | `si`   | 1 | Incluir [puntos BIP](#BIP-Spot) |
| `include_stop_routes`     | `si`   | 1 | Incluir [stop routes](#StopRoute) |
| `center_lon` | `si` | `-70.643562`      | Longitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lat`.    |
| `center_lat` | `si` | `-33.491585`      | Latitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lon`.     |
| `radius` | `si` | 2000      | Radio en metros. Usar en conjunto con `center_lat` y `center_lon`. Si es definido, se mostrarán sólo los resultados dentro de ese radio (en relación al centro (`center_lat` y `center_lon`)     |
| `bbox` | `si` |     |  Bounding box (min Longitude , min Latitude , max Longitude , max Latitude). Si es definido, se mostrarán sólo los resultados dentro de este bbox.    |

> **Respuesta**

Lista de elementos. Dependiendo de la llamada pueden ser [Stops](#Stop) y/o [Puntos BIP](#BIP-Spot) y/o [Buses](#Bus) 

> **Ejemplo**

- Consulta

```curl
/v2/map?include_buses=1&include_bip_spots=1&center_lon=-70.643562&center_lat=-33.491585
```

- Respuesta

```json
{
    "results": {
        "bip_spots": [
            {
                "bip_spot_lat": "-33.4899533888888",
                "bip_spot_code": "66366",
                "bip_spot_fantasy_name": "ALMACEN LA ESCALERA SAN JOAQUIN",
                "bip_spot_commune": "SAN JOAQUIN",
                "bip_spot_lon": "-70.6348931666666",
                "bip_spot_address": "ALCALDE PEDRO ALARCON 478",
                "bip_spot_entity": "Fullcarga",
                "bip_opening_time": null
            },
            {
                "bip_spot_lat": "-33.4896497619048",
                "bip_spot_code": "79567",
                "bip_spot_fantasy_name": "ANITA",
                "bip_spot_commune": "SAN MIGUEL",
                "bip_spot_lon": "-70.6410821428571",
                "bip_spot_address": "MARIA AUXILIADORA 702",
                "bip_spot_entity": "Fullcarga",
                "bip_opening_time": null
            }
        ],
        "stops": [
            {
                "stop_lat": "-33.492728459",
                "stop_code": "PH187",
                "stop_lon": "-70.640237998",
                "agency_id": "TS",
                "stop_id": "PH187",
                "stop_name": "PH187-Parada 1 / Paradero 8   Santa Rosa"
            },
            {
                "stop_lat": "-33.490277793",
                "stop_code": "PH132",
                "stop_lon": "-70.651118926",
                "agency_id": "TS",
                "stop_id": "PH132",
                "stop_name": "PH132-Parada 2 / Paradero 8   Gran Avenida"
            }
        ],
        "buses": [
            {
                "bus_plate_number": "BJFP-22",
                "operator_number": 16,
                "direction_id": 1,
                "bus_movement_orientation": 2,
                "added_at": "2017-12-11T22:12:16+00:00",
                "bus_lon": "-70.6509170532227",
                "route_id": "H05",
                "bus_speed": 30.9,
                "bus_lat": "-33.5000419616699",
                "captured_at": "2017-12-11T22:12:08+00:00"
            },
            {
                "bus_plate_number": "BJFT-94",
                "operator_number": 16,
                "direction_id": 1,
                "bus_movement_orientation": 0,
                "added_at": "2017-12-11T22:12:30+00:00",
                "bus_lon": "-70.6513748168945",
                "route_id": "301",
                "bus_speed": 0,
                "bus_lat": "-33.4909744262695",
                "captured_at": "2017-12-11T22:12:23+00:00"
            }
        ]
    }
}
```

## BIP Spots (`Puntos carga`)

Información sobre puntos carga tarjeta BIP

### Listar puntos de carga

> **Endpoint**

```curl
/v1/bip_spots
```

> **Query params**

| property  | opcional       | default         | description                                         |
| --------- | :-------:  | :-------------: | --------------------------------------------------- |
| `limit`  | `si`   | `50`       | Cantidad de resultados por página.               |
| `page`     | `si`   | `1` | Número de página |
| `center_lon` | `si` | `None`      | Longitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lat`. Ejemplo: `-70.643562`      |
| `center_lat` | `si` | `None`      | Latitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lon`. Ejemplo: `-33.491585`      |
| `radius` | `si` | `None`      | Radio en metros. Usar en conjunto con `center_lat` y `center_lon`. Si es definido, se mostrarán sólo los resultados dentro de ese radio (en relación al centro (center_lat` y `center_lon`))     |

> **Respuesta**

Lista de [Bip Spots](#bip-spot)

> **Ejemplo**

- Consulta

```curl
/v1/bip_spots?limit=3
```

- Respuesta

```json
{
    "has_next": true,
    "page_number": 1,
    "total_results": 676,
    "total_pages": 676,
    "results": [
        {
            "bip_spot_code": "60",
            "bip_spot_fantasy_name": "PCMA60",
            "bip_spot_commune": "SANTIAGO",
            "bip_spot_lat": "-33.44272566",
            "bip_spot_lon": "-70.644872",
            "bip_spot_address": "AV. LIB. BERNARDO OHIGGINS 622",
            "bip_spot_entity": "Serviestado",
            "bip_opening_time": "Lun a Vie 8:00 a 19:00 Sab 9:00 a 17:00"
        },
        {
            "bip_spot_lat": "-33.486436",
            "bip_spot_code": "61",
            "bip_spot_fantasy_name": "PCMA61",
            "bip_spot_commune": "MAIPU",
            "bip_spot_lon": "-70.750752",
            "bip_spot_address": "AV. AMERICO VESPUCIO NORTE CALETERA ORIENTE 51",
            "bip_spot_entity": "Serviestado",
            "bip_opening_time": "Lun a Vie 8:00 a 19:00 Sab 9:00 a 17:00"
        },
        {
            "bip_spot_lat": "-33.5332983565256",
            "bip_spot_code": "64",
            "bip_spot_fantasy_name": "PCMA 64",
            "bip_spot_commune": "LA CISTERNA",
            "bip_spot_lon": "-70.6630693155897",
            "bip_spot_address": "GRAN AV. JOSE MIGUEL CARRERA 8496",
            "bip_spot_entity": "Serviestado",
            "bip_opening_time": "Lun a Vie 8:00 a 19:00 Sab 9:00 a 17:00"
        }
    ],
    "page_size": 3
}
```

### Obtener Punto de carga BIP

> **Endpoint**

```curl
/v1/bip_spots/<bip_spot_code>
```

> **Argumentos**

  - `{string} bip_spot_code`: identificador de Punto de carga

> **Respuesta**

[Objeto de BIP Spot](#bip-spot)


> **Ejemplo**

- Consulta

```curl
/v1/bip_spots/60
```

- Respuesta

```json
{
    "bip_spot_lat": "-33.44272566",
    "bip_spot_code": "60",
    "bip_spot_fantasy_name": "PCMA60",
    "bip_spot_commune": "SANTIAGO",
    "bip_spot_lon": "-70.644872",
    "bip_spot_address": "AV. LIB. BERNARDO OHIGGINS 622",
    "bip_spot_entity": "Serviestado",
    "bip_opening_time": "Lun a Vie 8:00 a 19:00 Sab 9:00 a 17:00"
}
```

## Agencias
Son las empresas de transporte público que proporcionan los datos de este feed.

### Listar agencias

> **Endpoint**

```curl
/v1/agencies
```

> **Respuesta**

Lista de [Agencias](#Agency)

> **Ejemplo**

- Consulta

```curl
/v1/agencies
```

- Respuesta

```json
{
    "results": [
        {
            "agency_url": "http://www.transantiago.cl",
            "agency_name": "Transantiago",
            "agency_id": "TS"
        },
        {
            "agency_url": "http://www.metro.cl",
            "agency_name": "Metro de Santiago",
            "agency_id": "M"
        },
        {
            "agency_url": "http://www.trencentral.cl/bin/link.cgi/servicios/metrotren-nos/",
            "agency_name": "MetroTren Nos",
            "agency_id": "MT"
        }
    ]
}
```

## Buses

Información sobre buses del transantiago en operación. Esta información es obtenida en tiempo real utilizando el [Web Service de Posicionamiento](https://www.dtpm.cl/index.php/2013-04-24-14-09-09/datos-y-servicios) provisto por la dirección de transporte público metropolitano.

!> **NOTA**: La información de origen (Web service de posicionamiento) es actualizada con una frecuencia de entre 1 y 2 minutos.

### Listar buses en operación

> **Endpoint**

```curl
/v1/buses
```

> **Query params**

| property  | opcional       | default         | description                                         |
| --------- | :-------:  | :-------------: | --------------------------------------------------- |
| `limit`  | `si`   | `50`       | Cantidad de resultados por página.               |
| `page`     | `si`   | `1` | Número de página |
| `route_id`     | `si`   | None | Opción de filtrar resultados por route_id` |
| `direction_id`     | `si`   | None | Opción de filtrar resultados por `direction_id |
| `center_lon` | `si` | `None`      | Longitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lat`. Ejemplo: `-70.643562`      |
| `center_lat` | `si` | `None`      | Latitud. Si se define, los resultados serán ordenados de más cercano a más lejano a este punto. Usar en conjunto con `center_lon`. Ejemplo: `-33.491585`      |
| `radius` | `si` | `None`      | Radio en metros. Usar en conjunto con `center_lat` y `center_lon`. Si es definido, se mostrarán sólo los resultados dentro de ese radio (en relación al centro (center_lat` y `center_lon`))     |

> **Respuesta**

Lista de [Buses](#Bus)

> **Ejemplo**

- Consulta

```curl
/v1/buses?limit=3
```

- Respuesta

```json
{
    "has_next": true,
    "page_number": 1,
    "total_results": 903,
    "total_pages": 903,
    "results": [
        {
            "bus_plate_number": "BJFC-73",
            "operator_number": 5,
            "direction_id": 1,
            "bus_movement_orientation": 6,
            "added_at": "2017-11-11T17:23:26+00:00",
            "bus_lon": "-70.6349182128906",
            "route_id": "502",
            "bus_speed": 0,
            "bus_lat": "-33.4347496032715",
            "captured_at": "2017-11-11T17:23:21+00:00"
        },
        {
            "bus_plate_number": "ZB-6711",
            "operator_number": 4,
            "direction_id": 1,
            "bus_movement_orientation": 5,
            "added_at": "2017-11-11T17:23:16+00:00",
            "bus_lon": "-70.6487503051758",
            "route_id": "403",
            "bus_speed": 0,
            "bus_lat": "-33.4433441162109",
            "captured_at": "2017-11-11T17:23:09+00:00"
        },
        {
            "bus_plate_number": "CJRC-20",
            "operator_number": 9,
            "direction_id": 1,
            "bus_movement_orientation": 2,
            "added_at": "2017-11-11T17:23:11+00:00",
            "bus_lon": "-70.5545120239258",
            "route_id": "F06",
            "bus_speed": 0,
            "bus_lat": "-33.602611541748",
            "captured_at": "2017-11-11T17:23:06+00:00"
        }
    ],
    "page_size": 3
}
```

### Obtener bus

> **Endpoint**

```curl
/v1/buses/<bus_plate_number>
```

> **Argumentos**

  - `{string} bus_plate_number`: patente de bus

> **Respuesta**

[Objeto de Bus](#Bus)

> **Ejemplo**

- Consulta

```curl
/v1/buses/BJFC-73
```

- Respuesta

```json
{
    "bus_plate_number": "BJFC-73",
    "operator_number": 5,
    "direction_id": 1,
    "bus_movement_orientation": 6,
    "added_at": "2017-11-11T17:23:26+00:00",
    "bus_lon": "-70.6349182128906",
    "route_id": "502",
    "bus_speed": 0,
    "bus_lat": "-33.4347496032715",
    "captured_at": "2017-11-11T17:23:21+00:00"
}
```

# Objetos de respuestas

##  Direction (simplificada)
| campo  |  descripción  |
| --------- | :-------------: |
| `direction_id`  |  El campo direction_id contiene un valor binario que indica la dirección de un viaje. `0` : viaje de ida. `1`: viaje de regreso. |
| `direction_headsign`  |  Contiene el texto que aparece en un cartel que identifica el destino del viaje para los pasajeros.  |
| `direction_name`  |  Nombres para cada dirección con el campo trip_headsign. |


##  Direction
| campo  |  descripción  |
| --------- | :-------------: |
| `direction_id`  |  El campo direction_id contiene un valor binario que indica la dirección de un viaje. `0` : viaje de ida. `1`: viaje de regreso. |
| `direction_headsign`  |  Contiene el texto que aparece en un cartel que identifica el destino del viaje para los pasajeros.  |
| `direction_name`  |  Nombres para cada dirección con el campo trip_headsign. |
| `stop_times`  |  Lista de [stop times](#StopTime). |
| `shape`  |  Lista de [shapes](#Shape). |


## StopRoute

Tupla ([route](#Route), [dirección](#Direction-simplificada)) que contiene información sobre un recorrido en una dirección particular para una stop

| campo  |  descripción  |
| --------- | :-------------: |
| `route`  |  Objecto [route](#Route)   |
| `direction`  | Objecto [dirección](#Direction-simplificada) |
| `is_first_stop`  | Indica si este es el primer paradero para la ruta especificada. |
| `is_last_stop`  | Indica si este es paraderor terminal para la ruta especificada. |




##  Route

Rutas (recorridos) de transporte público. Una ruta es un grupo de viajes que se muestran a los usuarios como servicio independiente.

| campo  |  descripción  |
| --------- | :-------------: |
| `route_long_name`  | Contiene el nombre completo de una ruta. Este nombre suele ser más descriptivo que el route_short_name y suele incluir el destino o parada de la ruta.  |
| `end_date`  |  Contiene la fecha de finalización del servicio. Esta fecha se incluye en el intervalo del servicio. |
| `route_type`  | Describe el tipo de transporte público utilizado en una ruta. Los valores válidos para este campo son: `0`: tranvía, metro ligero. `1`: subterráneo, metro. `2`: tren. Utilizado para viajes de larga distancia. `3`: autobús. Utilizado para rutas en autobús de corta y larga distancia. `4`: transbordador, ferry. `5`: funicular. Utilizado para funiculares en superficie en donde el cable pasa por debajo del vehículo. `6`: cabina, vehículo suspendido de un cable. `7`: funicular. Cualquier sistema diseñado para recorridos con una gran inclinación.  |
| `route_text_color`  |  El campo route_text_color se puede usar para especificar un color legible para el texto incluido sobre un fondo del valor route_color. Este color se debe proporcionar en formato hexadecimal como, por ejemplo, FFD700. |
| `route_id`  | El campo route_id es un ID que identifica una ruta de forma exclusiva. |
| `agency_id`  |  ID que identifica de forma exclusiva a una empresa de transporte público. Un feed de transporte público puede representar datos de más de una empresa. El agency_id es un conjunto de datos único.  |
| `route_color`  | Define el color correspondiente a una ruta. Este color está en formato hexadecimal.   |
| `route_desc`  | Contiene una descripción de una ruta, opcional. |
| `directions`  |  lista de [direcciones](#Direction-simplificada) |
| `route_url`  |  Contiene la URL de una página web relativa a una ruta concreta.  |
| `route_short_name`  |  Contiene el nombre corto de una ruta. |
| `start_date`  |  Contiene la fecha de inicio del servicio. |

##  Frequency

Tiempo entre viajes para las rutas cuya frecuencia de servicio es variable.


| campo  |  descripción  |
| --------- | :-------------: |
| `start_time`  |  Especifica la hora a la que empieza el servicio con la frecuencia especificada. La hora se calcula como "mediodía menos 12 h" (lo que corresponde a la medianoche, excepto durante el período en el que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio. |
| `end_time`  |  El campo end_time especifica la hora a la que el servicio cambia de frecuencia (o bien finaliza) en la primera parada del viaje. La hora se calcula como "mediodía menos 12 h" (lo que corresponde a la medianoche, excepto durante el período en el que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio. |
| `headway_secs`  |  Indica el período de tiempo en segundos entre salidas desde la misma parada (tiempo entre viajes) para este tipo de viaje, durante el intervalo de tiempo especificado mediante start_time y end_time. |
| `exact_times  |  Determina si los viajes basados en frecuencias deben estar programados de manera exacta según la información especificada de tiempo entre viajes. Los valores válidos para este campo son:`0` o (vacío): los viajes basados en frecuencias no están programados de manera exacta. Este es el comportamiento predeterminado. `1: los viajes basados en frecuencias están programados de manera exacta.  |


##  Trip

Viajes para cada ruta. Un viaje es una secuencia de dos o más paradas que se produce en una hora específica.

| campo  |  descripción  |
| --------- | :-------------: |
| `direction_id`  |  Contiene un valor binario que indica la dirección de un viaje. Usa este campo para diferenciar viajes con dos direcciones con el mismo valor de route_id. `0` : viaje de ida. `1`: viaje de regreso  |
| `start_time`  |  Especifica la hora a la que empieza el servicio con la frecuencia especificada. La hora se calcula como "mediodía menos 12 h" (lo que corresponde a la medianoche, excepto durante el período en el que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio  |
| `end_time`  |  Especifica la hora a la que el servicio cambia de frecuencia (o bien finaliza) en la primera parada del viaje. La hora se calcula como "mediodía menos 12 h" (lo que corresponde a la medianoche, excepto durante el período en el que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio. |    
| `route_id`  |  ID que identifica una ruta de forma exclusiva. route_id es un conjunto de datos único. |
| `frequency`  | [frecuencia](#Frequency)  |
| `trip_headsign`  |   Contiene el texto que aparece en un cartel que identifica el destino del viaje para los pasajeros.  |
| `service_id`  | ID que identifica de forma exclusiva un conjunto de fechas en el que el servicio está disponible en una o más rutas.  |
| `trip_len` | |
| `trip_id` | Contiene un ID que identifica un viaje. trip_id es un conjunto de datos único. |
| `trip_short_name` | Contiene el texto que aparece en horarios y carteles para que los pasajeros identifiquen el viaje. |

##  Bus
| campo  |  descripción  |
| --------- | :-------------: |
| `bus_plate_number`  |  Patente del bus.  |
| `operator_number`  |  Identificador del concesionario.  |
| `direction_id`  |  Contiene un valor binario que indica la dirección de un viaje. Usa este campo para diferenciar viajes con dos direcciones con el mismo valor de route_id. `0` : viaje de ida. `1`: viaje de regreso  |
| `bus_movement_orientation`  |  Son 8 direcciones posibles que indican dirección general del movimiento instantáneo. Los valores posibles son: 0 = Norte, 1 = Noreste, 2 = Este, 3 = Sureste, 4 = Sur, 5 = Suroeste, 6 = Oeste, 7 = Noroeste.  |
| `added_at`  |  Fecha de inclusión de la base de datos  |
| `bus_lon`  |  Coordenada geográfica de la última transmisión, correspondiente a la proyección EPSG4326 - WSG 84  |
| `route_id`  |  El campo route_id es un ID que identifica una ruta de forma exclusiva.  |
| `bus_speed`  |   Velocidad instantánea en km/h redondeado a 1 decimal.  |
| `bus_lat`  |  Coordenada geográfica de la última transmisión, correspondiente a la proyección EPSG4326 - WSG 84.  |
| `captured_at`  |  Fecha de la transmisión del bus.  |


##  BIP Spot
| campo  |  descripción  |
| --------- | :-------------: |
| `bip_spot_code`  |  Identificador único del punto de carga BIP  |
| `bip_spot_fantasy_name`  |  Identificador del punto de carga. Normalmente es un nombre de fantasía identificable por los usuarios.  |
| `bip_spot_commune`  |  Comuna  |
| `bip_spot_lat`  |  Latitud de la ubicación del punto BIP |
| `bip_spot_lon`  |  Longitud de la ubicación del punt BIP |
| `bip_spot_address`  |  Dirección del punto de carga BIP  |
| `bip_spot_entity`  |  Nombre de la institución donde está ubicado el punto de carga  |
| `bip_opening_time  |  Descripción de los horarios de atención del punto BIP  |

##  Agency

Una o varias empresas de transporte público que proporcionan los datos de este feed.

| campo  |  descripción  |
| --------- | :-------------: |
| `agency_id`  |  ID que identifica de forma exclusiva a una empresa de transporte público.   |
| `agency_name`  |  Contiene el nombre completo de la empresa de transporte público.   |
| `agency_url`  |  Contiene la URL de la empresa de transporte público.  |


##  Shape

Reglas para el trazado de las líneas en un mapa que representen las rutas de una organización de transporte público.

| campo  |  descripción  |
| --------- | :-------------: |
| `shape_pt_lat`  |  Latitud de un punto de una forma con un ID de forma.  |
| `shape_pt_lon`  |  Longitud de un punto de una forma con un ID de forma. |
| `shape_id`  |  ID que identifica exclusivamente a una forma.  |
| `shape_pt_sequence`  |  Asocia la latitud y la longitud de un punto de una forma al orden secuencial que tienen a lo largo de la forma. Los valores de shape_pt_sequence son enteros no negativos y aumentan a lo largo del viaje.  |

##  Stop

Ubicaciones concretas en donde los vehículos recogen o dejan pasajeros.

| campo  |  descripción  |
| --------- | :-------------: |
| `stop_lat`  |  El campo stop_lat contiene la latitud de una parada o estación. El valor de este campo es una latitud WGS 84 válida. |
| `directions`  | lista de [direcciones](#Direction-simplificada) |
| `stop_lon`     |   Contiene la longitud de una parada o estación. El valor de este campo es una latitud WGS 84 válida entre -180 y 180. |
| `stop_code`     | Contiene texto corto o un número que identifica de forma exclusiva la parada de los pasajeros. Los códigos de parada se suelen usar en sistemas de información sobre transporte público para teléfonos o impresos en los carteles de paradas para facilitar a los usuarios la consulta de los horarios de parada o información en tiempo real sobre llegadas a una parada concreta. |
| `agency_id`     |  Identifica una empresa para la ruta especificada.  |
| `stop_id`     | ID que identifica de forma exclusiva a una parada o estación. |
| `stop_name`     |  Contiene el nombre de una parada o estación.  |


## StopTime

Horarios a los que un vehículo llega a una parada concreta y sale de ella en cada viaje.

| campo  |  descripción  |
| --------- | :-------------: |
| `stop_id`  | ID que identifica de forma exclusiva a una parada o estación. |
| `trip_id`  | Contiene un ID que identifica un viaje. trip_id es un conjunto de datos único. |
| `arrival_time`     |  Especifica la hora de llegada a una parada concreta correspondiente a un viaje específico de una ruta. La hora se calcula como "mediodía menos 12 h" (lo que corresponde a la medianoche, excepto durante el período en el que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio.  |
| `departure_time`     | especifica la hora de salida de una parada concreta correspondiente a un viaje específico en una ruta. La hora se calcula como "mediodía menos 12 h" (que corresponde a la medianoche, excepto los días en los que se aplica el cambio de horario de verano/invierno) al principio de la fecha de servicio. |
| `stop_sequence`     |  identifica el orden de las paradas en un viaje en concreto. Los valores de stop_sequence son enteros no negativos y aumentan durante el viaje.  |
| `stop_headsign`     | Contiene el texto que aparece en un cartel que identifica el destino del viaje para los pasajeros. |

## Arrival

| campo  |  descripción  |
| --------- | :-------------: |
| `bus_plate_number`  |  Patente del bus. |
| `arrival_estimation`  | Texto con descripción de estimación de tiempo |
| `calculated_at`  | Fecha en que la información fue obtenida |
| `route_id`  | Identificador de ruta |
| `direction_id`  |  El campo direction_id contiene un valor binario que indica la dirección de un viaje. `0` : viaje de ida. `1`: viaje de regreso. IMPORTANTE: Este campo puede ser null` |
| `is_live`  | Indica si la información fue obtenida utilizando el servicio de predicción (tiempo real). Este valor es actualmente siempre true |
| `bus_distance`  | Distancia en metros al paradero |


## Paginación
| campo  |  descripción  |
| --------- | :-------------: |
| has_next  |  Boleano que indica si hay más páginas de respuesta  |
| page_number  |  El número de página actual  |
| total_results  |  total de resultados  |
| total_pages  |  Número total de páginas  |
| results  |  lista de resultados  |
| page_size  |  cantidad de resultados en página  |
