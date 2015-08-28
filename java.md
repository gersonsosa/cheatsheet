Java
===================

Prueba de rendimiento en milisegundos entre dos lineas.

```
long start_time = System.nanoTime();
resp = GeoLocationService.getLocationByIp(ipAddress);
long end_time = System.nanoTime();
double difference = (end_time - start_time)/1e6;
```

JPA
===================

## Consultas ##
JPA tiene varias formas de traer objetos desde la base de datos:

 - NamedQueries: Son consultas (Nativas o **hql**) definidas en la clase anotada para JPA (objeto de modelo de dominio) son invocadas usando el nombre de la consulta en un método del repositorio. Ejemplo: 
```
@NamedQuery(name ="Evento.encontrarPorFecha"
		query = "SELECT e.id, e.descripcion, e.fecha"
		+ "FROM Evento e WHERE ...condición"
/*Opcional*/ resultSetMapping = "eventosPorUsuario",
		resultClass = Evento.class)
...
/* Clase repositorio */
@Query(name = "Evento.encontrarPorFecha")
public List<Evento> encontrarEventosFecha(...);
```
 - Consultas JPQL (@Query): Son consultas definidas en el lenguaje JPQL dentro de la anotación @Query del método del repositorio.
```
@Query("SELECT e FROM Evento e WHERE e.avaluo.id = :avaluoId")
public List<Evento> encontrarEventosPorAvaluo(
@Param("avaluoId") Long avaluoId);
```
- Consultas nativas: Se escriben en lenguaje SQL pueden ser definidas en una NamedQuery o en una anotación @Query.
```
@NamedNativeQueries({
    @NamedNativeQuery(name = "Evento.encontrarEventosUsuario",
        query = "SELECT e.evento_id, e.descripcion, e.fecha_hora_inicio,
             + " e.fecha_hora_fin" FROM avaluos.evento e"
             + " WHERE e.tipo_documento_usuario = :tipoIdentificacion"
             + " AND e.numero_documento_usuario = :numeroDocumento"
             + " AND e.fecha_hora_inicio > CURRENT_DATE"
             + " AND e.avaluo_id <> :avaluoId ",
        resultSetMapping = "eventosPorUsuario", resultClass = Evento.class)
})
```

----------


Recuperar objetos
-------------

Para recuperar objetos traidos por consulta JPA identifica en muchos casos la mejor forma de ejemplificar los objetos que se traen de la base de datos, sin embargo es posible personalizar en donde se ejemplificaran los datos resultado de la consulta, por ejemplo cuando es una consulta nativa o personalizada o se quiere ejemplificar un objeto diferente del objeto de dominio.

- Usando el constructor de una clase del proyecto: es posible ejemplificar un objeto no necesariamente de la lógica de dominio o que este anotado para uso de JPA o Hibernate asi:

```
@Query("SELECT NEW com.helio4.bancol.avaluos.dto.EventoDTO(e.id, e.descripcion, e.fechaHoraInicio, e.fechaHoraFin, e.avaluo.id, e.usuario.id.tipoDocumentoIdentificacion, e.usuario.id.numeroDocumento) FROM Evento e WHERE e.avaluo.id = :avaluoId")
```

Es importante notar que se debe usar el nombre completo de la clase incluyendo el paquete al que pertenece.

Este tipo de consultas son más sencillas y se usan cuando no es necesario traer todos los objetos de la clase de dominio su no solo algunos.