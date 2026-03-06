## 1. Gestión de Instancias (Encendido y Apagado)

Este es el grupo de comandos más importante para ti ahora que vives en Arequipa y no tienes una refrigeradora, pues te ayudará a controlar el gasto de energía (y dinero) en AWS.

- **Encender la instancia:** `aws ec2 start-instances --instance-ids i-06f4f6c94c6f6517c`.
    
- **Apagar la instancia (Stop):** `aws ec2 stop-instances --instance-ids i-06f4f6c94c6f6517c`.
    
    - _Nota:_ Usa `stop` para pausar el cobro por cómputo sin perder tus datos.
        
- **Reiniciar:** `aws ec2 reboot-instances --instance-ids i-06f4f6c94c6f6517c`.
    
- **Ver el estado actual:** `aws ec2 describe-instances --instance-ids i-06f4f6c94c6f6517c --query "Reservations[*].Instances[*].State.Name" --output text`.
- 
macbook@macbooks-MacBook-Pro ~ % ssh -i ~/.ssh/sanjuandedios.pem ec2-user@54.159.159.140

---

## 2. Gestión de Redes y Elastic IP

Dado que ya asociaste tu IP fija `54.159.159.140`, estos comandos te sirven para auditoría y control.

- **Listar tus Elastic IPs:** `aws ec2 describe-addresses`.
    
- **Ver reglas de seguridad (Firewall):** `aws ec2 describe-security-groups --group-ids sg-tu-id-aqui`.
    
    - _Propósito:_ Úsalo para verificar si el puerto 22 (SSH) o el puerto de tu aplicación (ej. 80 o 443 para Nginx) están abiertos.
        

---

## 3. Exploración de Recursos y Diagnóstico

Como programador, a veces olvidas qué recursos tienes activos en la región de Virginia (`us-east-1`).

- **Listar todas las instancias con sus IPs:** `aws ec2 describe-instances --query "Reservations[*].Instances[*].{ID:InstanceId,IP:PublicIpAddress,Estado:State.Name}" --output table`.
    
- **Verificar quién eres (Identidad):** `aws sts get-caller-identity`.
    
    - _Propósito:_ Te confirma que estás usando el usuario `mac-wil` que creamos en IAM.
        

---

## 4. Comandos de "Pánico" o Limpieza

Si alguna vez decides rehacer tu arquitectura o terminar el proyecto "Maria Analytics and Engineering".

- **Terminar (Eliminar permanentemente):** `aws ec2 terminate-instances --instance-ids i-tu-id`.
    
    - _Advertencia:_ Esto borra el servidor por completo; úsalo solo si ya no necesitas el proyecto.
        
- **Liberar una Elastic IP:** `aws ec2 release-address --allocation-id eipalloc-0af584d361c7b010b`.
    
    - _Importante:_ Debes liberarla si ya no la usas para que AWS deje de cobrarte la tarifa por IP no utilizada.
        

---

### Recomendación para tu flujo de trabajo en la UNSA:

Como usas una terminal Zsh en tu Mac (el símbolo `%` que mencionaste), puedes crear **alias** en tu archivo `~/.zshrc`. Por ejemplo, podrías configurar que al escribir `prender-mining` se ejecute el comando de `start-instances` automáticamente.

¿Te gustaría que te ayude a crear estos **alias** en tu MacBook para que no tengas que recordar estos comandos tan largos cada vez que vas a programar?