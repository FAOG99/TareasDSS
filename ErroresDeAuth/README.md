
# Errores Comunes en Autenticación y Autorización en APIs Web

## 1. Comparaciones de Valor Inseguras en Lenguajes de Programación
**Error:** Comparaciones de valor inseguras en lenguajes como JavaScript y PHP debido a su tipado débil.
**Impacto:** Permite a atacantes eludir la autenticación.
**Ejemplo de código:**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"username":"admin", "password": true}' http://example.com/login
```
**Fuente:** [Common authentication and authorization vulnerabilities](https://www.invicti.com/blog/web-security/how-to-avoid-authentication-and-authorization-vulnerabilities/)

## 2. Confiar en el Control de Acceso Basado en la Ruta
**Error:** Implementar control de acceso mediante verificación de rutas específicas.
**Impacto:** Posibilita ataques de bypass de autenticación.
**Ejemplo de código:**
```bash
curl http://example.com/admin/../dashboard
```
**Fuente:** [11 Common Authentication Vulnerabilities You Need to Know](https://www.strongdm.com/blog/authentication-vulnerabilities)

## 3. Confianza Equivocada en las Fuentes de Autenticación
**Error:** Confianza errónea en fuentes de autenticación a nivel de subdominio.
**Impacto:** Posibilidad de tomar control de cuentas de usuario.
**Ejemplo de código:**
```bash
curl -H "Cookie: session_token=<token>" http://subdomain.example.com
```
**Fuente:** [Most Common Authentication Vulnerabilities](https://goteleport.com/blog/authentication-vulnerabilities/)
