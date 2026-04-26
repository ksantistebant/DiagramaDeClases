### Justificación del Diseño UML

**1. Uso de Composición en Pedidos y Clientes:**
Se aplicó una relación de **composición** (rombo relleno) entre `Pedido` y `DetallePedido`, y entre `Cliente` y `Cuenta`. Esto se debe a que existe una dependencia de vida estricta. Un "detalle de pedido" (ej. 2 camisas) no tiene ningún sentido lógico ni puede existir en la base de datos sin estar amarrado a un "pedido" general. Si la clase padre muere, sus partes mueren con ella.

**2. Uso de Herencia en Productos y Pagos:**
Se utilizó la **herencia** (flecha de generalización) para aplicar el principio de reutilización (DRY). Todos los productos comparten atributos base como `nombre` y `precio`, pero solo los físicos requieren logística (`peso`, `dimensiones`), mientras que los digitales requieren enlaces (`licencia`). Al usar herencia, centralizamos los datos comunes en la clase padre y delegamos los datos específicos a las clases hijas. La misma lógica de eficiencia se aplicó a los métodos de pago.

**3. Impacto al eliminar un Pedido en el sistema:**
Según las relaciones trazadas en este diseño, si se elimina un objeto `Pedido`, se desencadenará una eliminación en cascada de sus `DetallePedido`, de su `Envio` y de su `Pago` (ya que dependen directamente de él). Sin embargo, el objeto `Cliente` y el objeto `Producto` del catálogo permanecerán completamente intactos, ya que su relación con el pedido es de simple asociación (el producto sigue existiendo en la tienda aunque el pedido se cancele).

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
        +actualizarPerfil()
        +realizarPedido()
    }

    class Cuenta {
        +String usuario
        +String contrasena
        +Date fecha_creacion
        +iniciarSesion()
        +cerrarSesion()
    }

    class Producto {
        +String codigo
        +String nombre
        +float precio
        +int stock
        +actualizarStock()
    }

    class ProductoDigital {
        +float tamano_MBytes
        +String licencia
        +generarEnlace()
    }

    class ProductoFisico {
        +float peso
        +String dimensiones
        +calcularFlete()
    }

    class Categoria {
        +String nombre
        +mostrarProductos()
    }

    class Pedido {
        +Date fecha
        +String estado
        +float total
        +calcularTotal()
        +cambiarEstado()
    }

    class DetallePedido {
        +int cantidad
        +float precio
        +calcularSubtotal()
    }

    class Pago {
        +float monto
        +Date fecha
        +String estado
        +procesarPago()
    }

    class PagoTarjeta {
        +String numero_tarjeta
        +autorizarConBanco()
    }

    class PagoTransferencia {
        +String cuenta_origen
        +verificarDeposito()
    }

    class PagoEfectivo {
        +float monto_entregado
        +calcularCambio()
    }

    class Envio {
        +String direccion
        +Date fecha
        +String transportista
        +String estado
        +actualizarRastreo()
    }
