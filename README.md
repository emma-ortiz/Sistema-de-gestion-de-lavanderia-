<img width="1343" height="964" alt="sistema-de-gestion-de-lavanderia-_1" src="https://github.com/user-attachments/assets/e95cdeec-449a-4d19-9e16-ff9f9b218154" />

# models.py (Django)

```python
from django.db import models

class ClienteLavanderia(models.Model):
    id_cliente = models.AutoField(primary_key=True, unique=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    direccion_recogida = models.CharField(max_length=255)
    direccion_entrega = models.CharField(max_length=255)
    fecha_registro = models.DateField()
    notas_cliente = models.TextField(null=True, blank=True)
    preferencias_lavado = models.TextField(null=True, blank=True)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class EmpleadoLavanderia(models.Model):
    id_empleado = models.AutoField(primary_key=True, unique=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    cargo = models.CharField(max_length=50)
    fecha_contratacion = models.DateField()
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    turno = models.CharField(max_length=50)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    dni = models.CharField(max_length=20)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


class ArticuloRopa(models.Model):
    id_articulo = models.AutoField(primary_key=True, unique=True)
    tipo_prenda = models.CharField(max_length=100)
    color = models.CharField(max_length=50)
    material = models.CharField(max_length=50)
    tamano = models.CharField(max_length=20)
    instrucciones_especiales = models.TextField(null=True, blank=True)
    costo_lavado_estandar = models.DecimalField(max_digits=5, decimal_places=2)
    estado_articulo = models.CharField(max_length=50)
    es_delicado = models.BooleanField(default=False)
    id_cliente = models.ForeignKey(ClienteLavanderia, on_delete=models.CASCADE)

    def __str__(self):
        return f"{self.tipo_prenda} ({self.id_articulo})"


class PedidoLavanderia(models.Model):
    id_pedido = models.AutoField(primary_key=True, unique=True)
    id_cliente = models.ForeignKey(ClienteLavanderia, on_delete=models.CASCADE)
    fecha_recepcion = models.DateTimeField()
    fecha_entrega_estimada = models.DateField()
    fecha_entrega_real = models.DateTimeField(null=True, blank=True)
    estado_pedido = models.CharField(max_length=50)
    total_pedido = models.DecimalField(max_digits=10, decimal_places=2)
    metodo_pago = models.CharField(max_length=50)
    id_empleado_recepcion = models.ForeignKey(EmpleadoLavanderia, on_delete=models.SET_NULL, null=True)
    comentarios_cliente = models.TextField(null=True, blank=True)

    def __str__(self):
        return f"Pedido #{self.id_pedido}"


class DetallePedidoLavanderia(models.Model):
    id_detalle = models.AutoField(primary_key=True, unique=True)
    id_pedido = models.ForeignKey(PedidoLavanderia, on_delete=models.CASCADE)
    id_articulo = models.ForeignKey(ArticuloRopa, on_delete=models.CASCADE)
    cantidad = models.IntegerField()
    tipo_servicio = models.CharField(max_length=50)
    costo_servicio_individual = models.DecimalField(max_digits=5, decimal_places=2)
    subtotal_item = models.DecimalField(max_digits=10, decimal_places=2)
    manchas_detectadas = models.TextField(null=True, blank=True)
    instrucciones_item = models.TextField(null=True, blank=True)

    def __str__(self):
        return f"Detalle {self.id_detalle}"


class MaquinaLavanderia(models.Model):
    id_maquina = models.AutoField(primary_key=True, unique=True)
    tipo_maquina = models.CharField(max_length=50)
    marca = models.CharField(max_length=100)
    modelo = models.CharField(max_length=100)
    capacidad_kg = models.DecimalField(max_digits=5, decimal_places=2)
    estado_operativo = models.CharField(max_length=50)
    ultima_revision = models.DateField()
    num_serie = models.CharField(max_length=50)
    es_lavadora = models.BooleanField(default=False)
    es_secadora = models.BooleanField(default=False)

    def __str__(self):
        return f"{self.tipo_maquina} - {self.marca} {self.modelo}"


class ReporteOperacional(models.Model):
    id_reporte = models.AutoField(primary_key=True, unique=True)
    fecha_reporte = models.DateField()
    id_empleado = models.ForeignKey(EmpleadoLavanderia, on_delete=models.CASCADE)
    num_pedidos_procesados = models.IntegerField()
    kg_ropa_procesada = models.DecimalField(max_digits=10, decimal_places=2)
    tiempo_inactividad_maquinas = models.IntegerField()
    observaciones_turno = models.TextField(null=True, blank=True)
    consumo_agua_litros = models.DecimalField(max_digits=10, decimal_places=2)

    def __str__(self):
        return f"Reporte {self.id_reporte} - {self.fecha_reporte}"
```


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
