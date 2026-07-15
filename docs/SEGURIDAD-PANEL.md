# Seguridad del Panel de Administración Allinkay

## Medidas de Seguridad Implementadas

### 1. Autenticación real con Netlify Identity
- El acceso se valida con Netlify Identity + Git Gateway (el mismo sistema usado por `/admin`)
- La verificación de credenciales ocurre en el servidor de Netlify, no en el navegador
- No hay contraseñas ni secretos almacenados en el código fuente
- Los usuarios se administran desde el panel de Netlify (Identity > Invite users)
- La sesión se controla con el widget oficial (`netlifyIdentity`), no con `sessionStorage` manual

### 2. Política de Seguridad de Contenido (CSP)
- Header emulado con meta tags para navegadores modernos
- Restricción de fuentes de scripts y estilos
- Bloqueo de framing (evita que la página se cargue en iframes externos)

### 3. Protección contra Ataques Comunes
- X-Content-Type-Options: nosniff
- X-Frame-Options: DENY
- X-XSS-Protection: 1; mode=block
- Referrer-Policy: strict-origin-when-cross-origin
- Limpieza de contraseña de memoria (setTimeout)

### 4. Protección de Sesión
- Sesión gestionada por el widget de Netlify Identity (JWT en almacenamiento propio del widget)
- Logout explícito invalida la sesión mediante `netlifyIdentity.logout()`
- Validación de autenticación en carga de página con el evento `init`

### 5. Protección de Archivos
- Archivo .htaccess con reglas de seguridad adicionales
- Bloqueo de acceso a archivos sensibles (.sql, .log, .env, etc.)
- Prevención de hotlinking desde dominios externos

---

## Riesgos Identificados y Mitigación

| Riesgo | Mitigación | Estado |
|--------|------------|--------|
| Contraseña expuesta en código | Reemplazado por Netlify Identity (sin secretos en cliente) | ✅ Resuelto |
| Autenticación falsa (solo JS) | Validación real en servidor de Netlify | ✅ Resuelto |
| Clickjacking | X-Frame-Options: DENY | ✅ Implementado |
| Secuestro/replay de sesión | Sesión gestionada por el widget oficial de Netlify | ✅ Implementado |
| Acceso sin invitación | Solo usuarios invitados desde Netlify Identity pueden entrar | ✅ Implementado |

---

## Recomendaciones para Producción

1. **Invitar solo al equipo autorizado** desde Netlify Identity (Site settings > Identity > Invite users)
2. **Usar HTTPS** obligatorio (redirección ya implementada en JS)
3. **Restringir registro abierto**: en Netlify Identity, dejar "Registration preferences" en "Invite only"
4. **Considerar 2FA** para acceso administrativo (soportado por algunos proveedores externos de Netlify Identity)
5. **Revisar periódicamente** la lista de usuarios con acceso en el panel de Netlify
6. **Habilitar logs de seguridad** en servidor

---

## Contacto de Seguridad

Para reportar vulnerabilidades, contactar al equipo de desarrollo.

---

*Última actualización: Julio 2026*
