# Diagrama de clases - DomiExpress

```mermaid
classDiagram
    class GeneradorConsecutivo {
        <<Singleton>>
        -int contador
        -GeneradorConsecutivo()
        +obtenerInstancia() GeneradorConsecutivo$
        +siguiente() String
    }

    class Pedido {
        <<Product del Builder / Prototype>>
        -String id
        -String cliente
        -TipoEntrega tipoEntrega
        -String direccion
        -List~ItemPedido~ items
        -String notas
        -int cupon
        -double propina
        -Pedido(...)
        +getId() String
        +calcularTotal() double
        +mostrarResumen() void
        +agregarItem(ItemPedido) void
        +clonar() Pedido
    }

    class Builder {
        <<Builder>>
        +conCliente(String) Builder
        +conTipoEntrega(TipoEntrega) Builder
        +conDireccion(String) Builder
        +agregarItem(ItemPedido) Builder
        +conNotas(String) Builder
        +conCupon(int) Builder
        +conPropina(double) Builder
        +construir() Pedido
    }

    class ItemPedido
    class TipoEntrega {
        <<enumeration>>
        RECOGER
        DOMICILIO
    }

    class ProcesadorPago {
        <<Creator - Factory Method>>
        +procesar(Pedido) void
        #crearPasarela() PasarelaPago*
    }
    class ProcesadorTarjeta {
        <<ConcreteCreator>>
    }
    class ProcesadorPSE {
        <<ConcreteCreator>>
    }
    class ProcesadorEfectivo {
        <<ConcreteCreator>>
    }
    class ProcesadorNequi {
        <<ConcreteCreator - R6>>
    }

    class PasarelaPago {
        <<interface - Product>>
        +nombre() String
        +cobrar(double) boolean
    }
    class PasarelaTarjeta {
        <<ConcreteProduct>>
    }
    class PasarelaPSE {
        <<ConcreteProduct>>
    }
    class PasarelaEfectivo {
        <<ConcreteProduct>>
    }
    class PasarelaNequi {
        <<ConcreteProduct - R6>>
    }

    Pedido +-- Builder : clase interna
    Builder ..> Pedido : crea
    Builder ..> GeneradorConsecutivo : pide numero al construir
    Pedido ..> GeneradorConsecutivo : clonar() pide numero
    Pedido o-- ItemPedido
    Pedido --> TipoEntrega

    ProcesadorPago <|-- ProcesadorTarjeta
    ProcesadorPago <|-- ProcesadorPSE
    ProcesadorPago <|-- ProcesadorEfectivo
    ProcesadorPago <|-- ProcesadorNequi
    ProcesadorPago ..> PasarelaPago : usa
    ProcesadorPago ..> Pedido : procesa

    PasarelaPago <|.. PasarelaTarjeta
    PasarelaPago <|.. PasarelaPSE
    PasarelaPago <|.. PasarelaEfectivo
    PasarelaPago <|.. PasarelaNequi

    ProcesadorTarjeta ..> PasarelaTarjeta : crea
    ProcesadorPSE ..> PasarelaPSE : crea
    ProcesadorEfectivo ..> PasarelaEfectivo : crea
    ProcesadorNequi ..> PasarelaNequi : crea
```
