```mermaid
classDiagram
    %% 1. Clientes y cuentas
    Cliente "1" *-- "1" Cuenta : Posee (Composición)
    Cliente "1" --> "*" Pedido : Realiza

    %% 2. Productos y catálogo
    Categoria "1" o-- "*" Producto : Agrupa (Agregación)
    Producto <|-- ProductoFisico : Herencia
    Producto <|-- ProductoDigital : Herencia

    %% 3. Pedidos
    Pedido "1" *-- "*" DetallePedido : Contiene (Composición)
    DetallePedido "*" --> "1" Producto : Incluye
    Pedido "1" --> "0..1" Envio : Genera (Solo físicos)

    %% 4. Pagos
    Pedido "1" --> "1" Pago : Tiene
    Pago <|-- PagoTarjeta : Herencia
    Pago <|-- PagoTransferencia : Herencia
    Pago <|-- PagoEfectivo : Herencia

    class Cliente {
        +String nombre
        +String correo_electronico
        +String telefono
    }

    class Cuenta {
        +String usuario
        +String contrasena
        +Date fecha_creacion
    }

    class Producto {
        +String codigo
        +String nombre
        +float precio
        +int stock
    }

    class ProductoDigital {
        +float tamano_MBytes
        +String licencia
    }

    class ProductoFisico {
        +float peso
        +String dimensiones
    }

    class Categoria {
        +String nombre
    }

    class Pedido {
        +Date fecha
        +String estado
        +float total
    }

    class DetallePedido {
        +int cantidad
        +float precio
    }

    class Pago {
        +float monto
        +Date fecha
        +String estado
    }

    class PagoTarjeta {
        +String numero_tarjeta
    }

    class PagoTransferencia {
        +String cuenta_origen
    }

    class PagoEfectivo {
        +float monto_entregado
    }

    class Envio {
        +String direccion
        +Date fecha
        +String transportista
        +String estado
    }
