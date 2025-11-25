# UIII-Act-7-modelo-Tienda-de-Ropa-No-Lista-4
# diseño de dbdesigner
**diseño de la relacion de tablas**
<img width="986" height="700" alt="image" src="https://github.com/user-attachments/assets/88f7faa0-b02d-4495-bde0-fa5dcc3f334f" />


# models.py

    from django.db import models
    
    
    class Categoria(models.Model):
        id_categoria = models.AutoField(primary_key=True)
        nombre_categoria = models.CharField(max_length=100)
        descripcion_categoria = models.TextField()
        fecha_creacion = models.DateTimeField(auto_now_add=True)
        ultima_actualizacion = models.DateTimeField(auto_now=True)
        es_activa = models.BooleanField(default=True)
    
        def __str__(self):
            return self.nombre_categoria
    
    
    class Marca(models.Model):
        id_marca = models.AutoField(primary_key=True)
        nombre_marca = models.CharField(max_length=100)
        pais_origen = models.CharField(max_length=50)
        sitio_web = models.CharField(max_length=255, blank=True, null=True)
        telefono_contacto = models.CharField(max_length=20, blank=True, null=True)
        fecha_registro = models.DateField(auto_now_add=True)
    
        def __str__(self):
            return self.nombre_marca
    
    
    class Producto(models.Model):
        id_producto = models.AutoField(primary_key=True)
        nombre_producto = models.CharField(max_length=255)
        descripcion = models.TextField()
        precio = models.DecimalField(max_digits=10, decimal_places=2)
        stock = models.IntegerField()
        id_categoria = models.ForeignKey(Categoria, on_delete=models.CASCADE, related_name='productos')
        id_marca = models.ForeignKey(Marca, on_delete=models.CASCADE, related_name='productos')
        talla = models.CharField(max_length=10)
        color = models.CharField(max_length=20)
        material = models.CharField(max_length=50)
    
        def __str__(self):
            return self.nombre_producto
    
    
    class Cliente_Ropa(models.Model):
        id_cliente = models.AutoField(primary_key=True)
        nombre = models.CharField(max_length=100)
        apellido = models.CharField(max_length=100)
        direccion = models.CharField(max_length=255)
        telefono = models.CharField(max_length=20)
        email = models.CharField(max_length=100)
        fecha_registro = models.DateField(auto_now_add=True)
        puntos_fidelidad = models.IntegerField(default=0)
        preferencias_estilo = models.TextField()
    
        def __str__(self):
            return f"{self.nombre} {self.apellido}"
    
    
    class Empleado_Tienda(models.Model):
        id_empleado = models.AutoField(primary_key=True)
        nombre = models.CharField(max_length=100)
        apellido = models.CharField(max_length=100)
        cargo = models.CharField(max_length=50)
        dni = models.CharField(max_length=20)
        fecha_contratacion = models.DateField()
        salario = models.DecimalField(max_digits=10, decimal_places=2)
        telefono = models.CharField(max_length=20)
        email = models.CharField(max_length=100)
    
        def __str__(self):
            return f"{self.nombre} {self.apellido}"
    
    
    class Venta_Ropa(models.Model):
        id_venta = models.AutoField(primary_key=True)
        fecha_venta = models.DateTimeField(auto_now_add=True)
        id_cliente = models.ForeignKey(Cliente_Ropa, on_delete=models.CASCADE, related_name='ventas')
        id_empleado = models.ForeignKey(Empleado_Tienda, on_delete=models.CASCADE, related_name='ventas')
        total_venta = models.DecimalField(max_digits=10, decimal_places=2)
        metodo_pago = models.CharField(max_length=50)
        descuento_total = models.DecimalField(max_digits=5, decimal_places=2, default=0)
        estado_venta = models.CharField(max_length=50)
    
        def __str__(self):
            return f"Venta #{self.id_venta}"
    
    
    class Detalle_Venta_Ropa(models.Model):
        id_detalle = models.AutoField(primary_key=True)
        id_venta = models.ForeignKey(Venta_Ropa, on_delete=models.CASCADE, related_name='detalles')
        id_producto = models.ForeignKey(Producto, on_delete=models.CASCADE, related_name='detalles_venta')
        cantidad = models.IntegerField()
        precio_unitario = models.DecimalField(max_digits=10, decimal_places=2)
        subtotal = models.DecimalField(max_digits=10, decimal_places=2)
        iva_aplicado = models.DecimalField(max_digits=5, decimal_places=2)
        id_talla_vendida = models.CharField(max_length=10)
    
        def __str__(self):
            return f"Detalle #{self.id_detalle} de Venta {self.id_venta.id_venta}"
