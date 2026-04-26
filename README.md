classDiagram
    %% 1. RELACIONES DE USUARIOS
    %% Composición: Si el cliente desaparece, su cuenta también.
    Cliente "1" *-- "1" Cuenta : Posee
    Cliente "1" --> "*" Pedido : Realiza

    %% 2. RELACIONES DE CATÁLOGO
    %% Agregación: La categoría agrupa productos, pero los productos son independientes.
    Categoria "1" o-- "*" Producto : Agrupa
    %% Herencia: Tipos de productos
    Producto <|-- ProductoFisico : Es un
    Producto <|-- ProductoDigital : Es un

    %% 3. RELACIONES DE VENTAS Y ENVÍOS
    %% Composición: Un detalle de pedido no tiene sentido sin el pedido general.
    Pedido "1" *-- "*" DetallePedido : Contiene
    DetallePedido "*" --> "1" Producto : Incluye
    Pedido "1" --> "1" Envio : Requiere

    %% 4. RELACIONES DE PAGOS
    Pedido "1" --> "1" Pago : Genera
    %% Herencia: Tipos de pagos
    Pago <|-- PagoTarjeta : Es un
    Pago <|-- PagoTransferencia : Es un
    Pago <|-- PagoEfectivo : Es un

    %% DEFINICIÓN DE CLASES Y ATRIBUTOS
    class Cliente {
        +int id_cliente
        +String nombre
        +String direccion
        +String email
        +String telefono
        +actualizarPerfil()
        +realizarPedido()
    }

    class Cuenta {
        +String usuario
        +String contrasena
        +Date fecha_registro
        +boolean estado_activa
        +iniciarSesion()
        +cerrarSesion()
        +restablecerClave()
    }

    class Categoria {
        +int id_categoria
        +String nombre
        +String descripcion
        +agregarProducto()
        +mostrarProductos()
    }

    class Producto {
        +int id_producto
        +String nombre
        +float precio_base
        +String descripcion
        +obtenerPrecio()
        +actualizarInfo()
    }

    class ProductoFisico {
        +float peso
        +String dimensiones
        +float costo_flete
        +calcularEnvio()
    }

    class ProductoDigital {
        +float tamano_archivo
        +String formato
        +String enlace_descarga
        +generarEnlace()
    }

    class Pedido {
        +int id_pedido
        +Date fecha
        +String estado
        +float total
        +calcularTotal()
        +confirmar()
        +cancelar()
    }

    class DetallePedido {
        +int cantidad
        +float precio_unitario
        +float subtotal
        +calcularSubtotal()
    }

    class Envio {
        +int id_envio
        +String empresa_transporte
        +String numero_rastreo
        +String estado_entrega
        +actualizarEstado()
        +rastrear()
    }

    class Pago {
        +int id_pago
        +float monto
        +Date fecha
        +String estado
        +procesarPago()
    }

    class PagoTarjeta {
        +String numero_tarjeta
        +String fecha_expiracion
        +String titular
        +autorizarConBanco()
    }

    class PagoTransferencia {
        +String banco_origen
        +String numero_cuenta
        +String codigo_referencia
        +verificarDeposito()
    }

    class PagoEfectivo {
        +float monto_entregado
        +float cambio
        +calcularCambio()
    }