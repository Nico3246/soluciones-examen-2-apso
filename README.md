## Documento 2 – Soluciones para “wuolah‐free‐PruebashellL11920.pdf” (Grupo L1)

_Este documento contiene una propuesta de solución para la prueba del Grupo L1. Se asume que la estructura se crea en `~/pruebaL1`._

1. **Cambiar el passwd de la cuenta:**  
   ```bash
   passwd
   ```

2. **Crear la estructura de directorios:**  
   (Se requiere: `pruebaL1`, y en ella: `ficheros`, `guiones`, `trash` y `temporal`.)
   ```bash
   mkdir -p /home/usuario/pruebaL1/{ficheros,guiones,trash,temporal}
   ```

3. **Modificar permisos:**  
   - Para el directorio `pruebaL1` (sin permisos para grupo y otros):
     ```bash
     chmod 700 /home/usuario/pruebaL1
     ```
   - Para el directorio `ficheros` (permisos: rwxr-xr--, es decir, 754):
     ```bash
     chmod 754 /home/usuario/pruebaL1/ficheros
     ```

4. **Copiar ficheros desde `/home/so/ficheros`:**  
   Copiar aquellos cuyo nombre tenga un número en la tercera posición:
   ```bash
   cp /home/so/ficheros/??[0-9]* /home/usuario/pruebaL1/ficheros
   ```

5. **Mover ficheros de `ficheros` que terminen en “bak” a `trash`:**  
   ```bash
   mv /home/usuario/pruebaL1/ficheros/*bak /home/usuario/pruebaL1/trash
   ```

6. **Borrar el directorio `trash`:**  
   ```bash
   rmdir /home/usuario/pruebaL1/trash
   ```
   (O usar `rm -r` si no está vacío.)

7. **Incorporar al PATH la ruta del directorio de trabajo:**  
   (Por ejemplo, añadiendo a `.bashrc`)
   ```bash
   export PATH=$PATH:$(pwd)
   ```

8. **Crear variable y alias al iniciar sesión:**  
   En `.bashrc`, agregar:
   ```bash
   export GUIONES=/home/usuario/pruebaL1/guiones
   ```
   y
   ```bash
   alias cdvarios='cd /home/usuario/pruebaL1/ficheros'
   ```

9. **Encontrar enlaces simbólicos en `/usr`:**  
   Buscar los enlaces cuyo nombre contenga “head”, modificados hace más de 10 días y de tamaño superior a 60 bytes (aproximadamente 30 palabras de 2 bytes cada una), y guardar la salida en dos ficheros:
   ```bash
   find /usr -type l -name '*head*' -mtime +10 -size +60c -exec echo "Fichero: {}" \; > /home/usuario/pruebaL1/ficheros/elfind 2> /home/usuario/pruebaL1/ficheros/errorfind
   ```

10. **Añadir la hora actual al final del fichero `elfind`:**  
    ```bash
    date >> /home/usuario/pruebaL1/ficheros/elfind
    ```

11. **Crear enlace simbólico en `temporal` llamado `noacces` que apunte a `errorfind`:**  
    ```bash
    ln -s /home/usuario/pruebaL1/ficheros/errorfind /home/usuario/pruebaL1/temporal/noacces
    ```

12. **Listar, en orden inverso, usuarios conectados cuyo nombre contenga “va”:**  
    Guardar la salida en `p12.txt` en `ficheros`:
    ```bash
    who | grep 'va' | sort -r > /home/usuario/pruebaL1/ficheros/p12.txt
    ```

13. **Borrar, con una sola orden, todos los ficheros que en el directorio de usuario terminen en “0”:**  
    ```bash
    find ~ -type f -name '*0' -delete
    ```

14. **Crear un fichero de texto (`elgrub.txt`) en `ficheros` con la línea del fichero de configuración de nuevos usuarios que indique el shell por defecto:**  
    ```bash
    grep '^SHELL' /etc/default/useradd > /home/usuario/pruebaL1/ficheros/elgrub.txt
    ```

15. **Buscar en la caché de paquetes los relacionados con “sgb”:**  
    ```bash
    apt-cache search sgb > /home/usuario/pruebaL1/ficheros/paquetes.txt
    ```

16. **Visualizar la cuota de disco y guardarla en `ocupacion.txt` en `ficheros`:**  
    ```bash
    quota -v > /home/usuario/pruebaL1/ficheros/ocupacion.txt
    ```

17. **Mostrar el estado del servicio `webmin.service` y guardarlo en `estado.txt` en `ficheros`:**  
    ```bash
    systemctl status webmin.service > /home/usuario/pruebaL1/ficheros/estado.txt
    ```

18. **Crear un script en `guiones` llamado `g1`:**  
    ```bash
    #!/bin/bash
    read -p "Ingrese un nombre: " DIR
    mkdir "$1/$DIR"
    alias minombre="echo 'Tu Nombre Real' > $1/$DIR/$2"
    ```
    Hacerlo ejecutable:
    ```bash
    chmod +x /home/usuario/pruebaL1/guiones/g1
    ```

19. **Ejecutar el script `g1` pasando como parámetros `$HOME/pruebaL1/ficheros` y `estudiante.txt` y luego ejecutar el alias `minombre`:**  
    ```bash
    /home/usuario/pruebaL1/guiones/g1 $HOME/pruebaL1/ficheros estudiante.txt
    minombre
    ```

20. **Crear un script en `guiones` llamado `g2`:**  
    ```bash
    #!/bin/bash
    if [ "$#" -ne 2 ] || [ ! -d "$1" ]; then
      echo "Error: parámetros incorrectos"
      exit 1
    fi
    TOTAL=0
    for file in "$1"/*; do
      if [[ $(basename "$file") == *"$2"* ]]; then
        cat "$file"
      fi
      if [[ $(basename "$file") =~ ^f.. ]]; then
        ((TOTAL++))
      elif [[ $(basename "$file") =~ [0-9]$ ]]; then
        ((TOTAL++))
      else
        ((TOTAL--))
      fi
    done
    echo "TOTAL: $TOTAL"
    ```
    Hacerlo ejecutable:
    ```bash
    chmod +x /home/usuario/pruebaL1/guiones/g2
    
