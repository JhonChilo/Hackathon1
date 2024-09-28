# Hackathon: Sistema de Venta de Entradas para Cine 🎥

**Contexto:**  
Los cines son esos lugares mágicos que guardan grandes recuerdos, ya sea por la emoción de ver nuestra película favorita por primera vez 🍿, disfrutar de clásicos o compartir momentos con familia y amigos 🎬. En UTEC, también disfrutamos de noches de cine gracias a las organizaciones estudiantiles 🏫.

**Desafío:**  
Tu misión será diseñar un sistema de venta de entradas que permita a estas organizaciones gestionar eventos de cine de manera eficiente 📋. El sistema deberá permitir organizar y monitorear la recaudación de fondos y manejar los accesos de manera correcta 🔑.

**Requerimientos Específicos:**

1. **Entidades del Sistema:**
    - Estudiante 👤(nombre, email, codigo de estudiante)
    - Función 📽️ (nombre, descripción, fecha, duracion, precio, stock)
    - Ticket 🎟️ (id, qr, estado, estudiante, función, fecha de compra, cantidad de entradas)

2. **Funcionalidad de Tickets:**
    - Cada compra de entrada debe generar un ticket que incluya un QR como identificador único. Este QR se enviará al correo del usuario junto con un correo de plantilla 📧.
    - El sistema deberá cambiar el estado del ticket a "canjeado" una vez que se escanee la entrada ✔️ y enviar un correo de confirmación al usuario.

3. **Integración con API Externa:**
    - Utilizar la API de QR Code de [goQR.me](https://goqr.me/api/) para generar los códigos QR que se incluirán en los correos electrónicos de los tickets 🔗.

4. **Validación de Entradas:**
    - Implementar un endpoint que permita validar la autenticidad de un ticket escaneando el código QR. Si el ticket es válido, se deberá cambiar el estado del ticket a "canjeado" y enviar un correo de confirmación al usuario 📧. 

5. **Recaudación de Fondos:**
    - Implementar un endpoint que permita calcular la recaudación total de un evento en base a la cantidad de entradas vendidas y el precio de cada entrada 💰.

A continuación, se presenta un diagrama de clases que describe las entidades del sistema y sus relaciones en mermaid:

```mermaid
classDiagram
direction BT
class Estado {
<<enumeration>>
  + Estado() 
  +  CANJEADO
  +  VENDIDO
}
class Estudiante {
  + Estudiante(Long, String, String, Long, List~Ticket~) 
  + Estudiante() 
  - Long id
  - String name
  - Long studentCode
  - String email
  - List~Ticket~ tickets
}
class Funcion {
  + Funcion(Long, String, String, LocalDateTime, Integer, Double, Integer, List~Ticket~) 
  + Funcion() 
  - String descripcion
  - Integer stock
  - Long id
  - List~Ticket~ tickets
  - LocalDateTime fecha
  - Integer duracion
  - String nombre
  - Double precio
}
class Ticket {
  + Ticket() 
  + Ticket(Long, String, Estado, Funcion, Estudiante, LocalDateTime, Integer) 
  - Integer cantidad
  - Funcion funcion
  - LocalDateTime fechaCompra
  - String qr
  - Estudiante estudiante
  - Long id
  - Estado estado
}

Estudiante "1" *--> "tickets *" Ticket 
Funcion "1" *--> "tickets *" Ticket 
Ticket "1" *--> "estado 1" Estado 
Ticket "1" *--> "estudiante 1" Estudiante 
Ticket "1" *--> "funcion 1" Funcion 
```