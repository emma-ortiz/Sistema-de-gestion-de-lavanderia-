<img width="1343" height="964" alt="sistema-de-gestion-de-lavanderia-_1" src="https://github.com/user-attachments/assets/e95cdeec-449a-4d19-9e16-ff9f9b218154" />

```sql
cliente_lavanderia {
    id_cliente integer pk increments unique
    nombre varchar(100)
    apellido varchar(100)
    telefono varchar(20)
    email varchar(100)
    direccion_recogida varchar(255)
    direccion_entrega varchar(255)
    fecha_registro date
    notas_cliente text
    preferencias_lavado text
}

empleado_lavanderia {
    id_empleado integer pk increments unique
    nombre varchar(100)
    apellido varchar(100)
    cargo varchar(50)
    fecha_contratacion date
    salario decimal(10,2)
    turno varchar(50)
    telefono varchar(20)
    email varchar(100)
    dni varchar(20)
}

articulo_ropa {
    id_articulo integer pk increments unique
    tipo_prenda varchar(100)
    color varchar(50)
    material varchar(50)
    tamano varchar(20)
    instrucciones_especiales text
    costo_lavado_estandar decimal(5,2)
    estado_articulo varchar(50)
    es_delicado boolean
    id_cliente integer fk
}

pedido_lavanderia {
    id_pedido integer pk increments unique
    id_cliente integer fk
    fecha_recepcion datetime
    fecha_entrega_estimada date
    fecha_entrega_real datetime
    estado_pedido varchar(50)
    total_pedido decimal(10,2)
    metodo_pago varchar(50)
    id_empleado_recepcion integer fk
    comentarios_cliente text
}

detalle_pedido_lavanderia {
    id_detalle integer pk increments unique
    id_pedido integer fk
    id_articulo integer fk
    cantidad integer
    tipo_servicio varchar(50)
    costo_servicio_individual decimal(5,2)
    subtotal_item decimal(10,2)
    manchas_detectadas text
    instrucciones_item text
}

maquina_lavanderia {
    id_maquina integer pk increments unique
    tipo_maquina varchar(50)
    marca varchar(100)
    modelo varchar(100)
    capacidad_kg decimal(5,2)
    estado_operativo varchar(50)
    ultima_revision date
    num_serie varchar(50)
    es_lavadora boolean
    es_secadora boolean
}

reporte_operacional {
    id_reporte integer pk increments unique
    fecha_reporte date
    id_empleado integer fk
    num_pedidos_procesados integer
    kg_ropa_procesada decimal(10,2)
    tiempo_inactividad_maquinas integer
    observaciones_turno text
    consumo_agua_litros decimal(10,2)
}
```
