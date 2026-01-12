# Sistema Hotelero - Documentación de Base de Datos

## Resumen
- **Total de Tablas:** 32
- **Motor:** MySQL
- **Codificación:** UUID para PKs

---

## Tablas y Estructura

### 1. hoteles
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_hotel | CHAR(36) | NO | UUID() | PK |
| nombre | VARCHAR(200) | NO | | |
| razon_social | VARCHAR(200) | SI | | |
| rfc | VARCHAR(20) | SI | | |
| direccion | TEXT | SI | | |
| ciudad | VARCHAR(100) | SI | | |
| estado | VARCHAR(100) | SI | | |
| pais | VARCHAR(100) | SI | 'México' | |
| codigo_postal | VARCHAR(10) | SI | | |
| telefono | VARCHAR(20) | SI | | |
| email | VARCHAR(100) | SI | | |
| sitio_web | VARCHAR(200) | SI | | |
| logo_url | VARCHAR(500) | SI | | |
| moneda | VARCHAR(3) | SI | 'MXN' | |
| zona_horaria | VARCHAR(50) | SI | 'America/Mexico_City' | |
| hora_checkin | TIME | SI | '15:00:00' | |
| hora_checkout | TIME | SI | '12:00:00' | |
| politica_cancelacion | TEXT | SI | | |
| politica_ninos | TEXT | SI | | |
| politica_mascotas | TEXT | SI | | |
| terminos_condiciones | TEXT | SI | | |
| estrellas | TINYINT | SI | 3 | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

---

### 2. usuarios
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_usuario | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| email | VARCHAR(100) | NO | | UNIQUE |
| password_hash | VARCHAR(255) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| apellido_paterno | VARCHAR(100) | SI | | |
| apellido_materno | VARCHAR(100) | SI | | |
| telefono | VARCHAR(20) | SI | | |
| rol | ENUM | NO | | SuperAdmin,Admin,Gerente,Recepcion,Housekeeping,Mantenimiento,Contador |
| foto_url | VARCHAR(500) | SI | | |
| ultimo_acceso | DATETIME | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 3. catalogo_estados
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_estado | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| categoria | ENUM | NO | | Reserva,Habitacion,Limpieza,Mantenimiento,Ocupacion,Pago |
| codigo | VARCHAR(50) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| color | VARCHAR(7) | SI | '#6B7280' | |
| icono | VARCHAR(50) | SI | | |
| orden | INT | SI | 0 | |
| es_default | BOOLEAN | SI | FALSE | |
| activo | BOOLEAN | SI | TRUE | |

**Índices únicos:** `(id_hotel, categoria, codigo)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 4. catalogo_metodos_pago
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_metodo | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| codigo | VARCHAR(50) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| requiere_referencia | BOOLEAN | SI | FALSE | |
| comision_porcentaje | DECIMAL(5,2) | SI | 0 | |
| comision_fija | DECIMAL(10,2) | SI | 0 | |
| cuenta_contable | VARCHAR(50) | SI | | |
| activo | BOOLEAN | SI | TRUE | |

**Índices únicos:** `(id_hotel, codigo)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 5. catalogo_origen_reserva
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_origen | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| codigo | VARCHAR(50) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| comision_porcentaje | DECIMAL(5,2) | SI | 0 | |
| url_integracion | VARCHAR(500) | SI | | |
| activo | BOOLEAN | SI | TRUE | |

**Índices únicos:** `(id_hotel, codigo)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 6. catalogo_categorias_producto
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_categoria | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| codigo | VARCHAR(50) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| tipo | ENUM | NO | | Hospedaje,Servicio,Producto,Consumo |
| activo | BOOLEAN | SI | TRUE | |

**Índices únicos:** `(id_hotel, codigo)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 7. catalogo_tipos_documento
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_tipo_doc | CHAR(36) | NO | UUID() | PK |
| codigo | VARCHAR(20) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| pais | VARCHAR(100) | SI | | |
| activo | BOOLEAN | SI | TRUE | |

---

### 8. impuestos
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_impuesto | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| nombre | VARCHAR(100) | NO | | |
| codigo | VARCHAR(20) | SI | | |
| tipo | ENUM | NO | | Porcentaje,Fijo |
| valor | DECIMAL(10,4) | NO | | |
| aplica_a | ENUM | SI | 'Todo' | Hospedaje,Servicios,Productos,Todo |
| es_retencion | BOOLEAN | SI | FALSE | |
| cuenta_contable | VARCHAR(50) | SI | | |
| orden_calculo | INT | SI | 0 | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 9. tipos_habitacion
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_tipo | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| codigo | VARCHAR(20) | NO | | |
| nombre | VARCHAR(100) | NO | | |
| descripcion | TEXT | SI | | |
| capacidad_adultos | INT | SI | 2 | |
| capacidad_ninos | INT | SI | 2 | |
| capacidad_maxima | INT | SI | 4 | |
| precio_base | DECIMAL(12,2) | NO | | |
| precio_persona_extra | DECIMAL(12,2) | SI | 0 | |
| tamano_m2 | DECIMAL(6,2) | SI | | |
| tipo_cama | VARCHAR(100) | SI | | |
| cantidad_camas | INT | SI | 1 | |
| amenidades | JSON | SI | | |
| fotos | JSON | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Índices únicos:** `(id_hotel, codigo)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`

---

### 10. habitaciones
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_habitacion | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_tipo | CHAR(36) | NO | | FK → tipos_habitacion |
| numero | VARCHAR(20) | NO | | |
| piso | INT | SI | | |
| edificio | VARCHAR(50) | SI | | |
| vista | ENUM | SI | 'Interior' | Mar,Jardin,Piscina,Ciudad,Interior,Montaña |
| orientacion | ENUM | SI | NULL | Norte,Sur,Este,Oeste |
| tiene_balcon | BOOLEAN | SI | FALSE | |
| tiene_terraza | BOOLEAN | SI | FALSE | |
| es_fumador | BOOLEAN | SI | FALSE | |
| accesible_discapacidad | BOOLEAN | SI | FALSE | |
| extension_telefonica | VARCHAR(10) | SI | | |
| wifi_password | VARCHAR(50) | SI | | |
| caja_fuerte_codigo | VARCHAR(20) | SI | | |
| notas_internas | TEXT | SI | | |
| estado_habitacion | CHAR(36) | SI | | FK → catalogo_estados |
| estado_limpieza | CHAR(36) | SI | | FK → catalogo_estados |
| estado_mantenimiento | CHAR(36) | SI | | FK → catalogo_estados |
| ultima_limpieza | DATETIME | SI | | |
| ultimo_mantenimiento | DATETIME | SI | | |
| proxima_mantenimiento | DATE | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

**Índices únicos:** `(id_hotel, numero)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_tipo` → `tipos_habitacion.id_tipo`
- `estado_habitacion` → `catalogo_estados.id_estado`
- `estado_limpieza` → `catalogo_estados.id_estado`
- `estado_mantenimiento` → `catalogo_estados.id_estado`

---

### 11. habitacion_inventario
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id | CHAR(36) | NO | UUID() | PK |
| id_habitacion | CHAR(36) | NO | | FK → habitaciones |
| articulo | VARCHAR(100) | NO | | |
| cantidad | INT | SI | 1 | |
| estado | ENUM | SI | 'Bueno' | Bueno,Regular,Malo,Reemplazar |
| fecha_ultimo_revision | DATE | SI | | |
| notas | TEXT | SI | | |

**Relaciones:**
- `id_habitacion` → `habitaciones.id_habitacion`

---

### 12. tarifas_habitacion
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_tarifa | CHAR(36) | NO | UUID() | PK |
| id_tipo | CHAR(36) | NO | | FK → tipos_habitacion |
| nombre | VARCHAR(100) | SI | | |
| fecha_inicio | DATE | NO | | |
| fecha_fin | DATE | NO | | |
| precio | DECIMAL(12,2) | NO | | |
| precio_persona_extra | DECIMAL(12,2) | SI | | |
| minimo_noches | INT | SI | 1 | |
| aplica_dias | JSON | SI | | |
| tipo_tarifa | ENUM | SI | 'Temporada' | Temporada,Promocion,Evento,LastMinute,EarlyBird |
| codigo_promo | VARCHAR(50) | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_tipo` → `tipos_habitacion.id_tipo`

---

### 13. clientes
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_cliente | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| tipo_cliente | ENUM | SI | 'Persona' | Persona,Empresa |
| nombre | VARCHAR(100) | NO | | |
| apellido_paterno | VARCHAR(100) | SI | | |
| apellido_materno | VARCHAR(100) | SI | | |
| nombre_comercial | VARCHAR(200) | SI | | |
| tipo_documento | CHAR(36) | SI | | FK → catalogo_tipos_documento |
| numero_documento | VARCHAR(50) | SI | | |
| nacionalidad | VARCHAR(100) | SI | | |
| fecha_nacimiento | DATE | SI | | |
| genero | ENUM | SI | 'No especificado' | M,F,Otro,No especificado |
| email | VARCHAR(100) | SI | | |
| telefono | VARCHAR(20) | SI | | |
| telefono_alternativo | VARCHAR(20) | SI | | |
| whatsapp | VARCHAR(20) | SI | | |
| direccion | TEXT | SI | | |
| ciudad | VARCHAR(100) | SI | | |
| estado | VARCHAR(100) | SI | | |
| pais | VARCHAR(100) | SI | | |
| codigo_postal | VARCHAR(10) | SI | | |
| rfc | VARCHAR(20) | SI | | |
| razon_social | VARCHAR(200) | SI | | |
| regimen_fiscal | VARCHAR(10) | SI | | |
| uso_cfdi | VARCHAR(10) | SI | 'G03' | |
| direccion_fiscal | TEXT | SI | | |
| email_facturacion | VARCHAR(100) | SI | | |
| empresa | VARCHAR(200) | SI | | |
| puesto | VARCHAR(100) | SI | | |
| preferencias_habitacion | JSON | SI | | |
| restricciones_alimentarias | TEXT | SI | | |
| alergias | TEXT | SI | | |
| notas_especiales | TEXT | SI | | |
| nivel_lealtad | ENUM | SI | 'Bronce' | Bronce,Plata,Oro,Platino,Diamante |
| puntos_acumulados | INT | SI | 0 | |
| fecha_membresia | DATE | SI | | |
| contacto_emergencia_nombre | VARCHAR(200) | SI | | |
| contacto_emergencia_telefono | VARCHAR(20) | SI | | |
| contacto_emergencia_relacion | VARCHAR(50) | SI | | |
| es_vip | BOOLEAN | SI | FALSE | |
| en_lista_negra | BOOLEAN | SI | FALSE | |
| motivo_lista_negra | TEXT | SI | | |
| total_estancias | INT | SI | 0 | |
| total_gastado | DECIMAL(14,2) | SI | 0 | |
| ultima_estancia | DATE | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `tipo_documento` → `catalogo_tipos_documento.id_tipo_doc`

---

### 14. reservas
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_reserva | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| numero_reserva | VARCHAR(20) | NO | | |
| id_cliente | CHAR(36) | NO | | FK → clientes |
| fecha_reserva | DATETIME | SI | CURRENT_TIMESTAMP | |
| fecha_checkin | DATE | NO | | |
| fecha_checkout | DATE | NO | | |
| hora_llegada_estimada | TIME | SI | | |
| id_habitacion | CHAR(36) | SI | | FK → habitaciones |
| id_tipo_habitacion | CHAR(36) | NO | | FK → tipos_habitacion |
| adultos | INT | SI | 1 | |
| ninos | INT | SI | 0 | |
| infantes | INT | SI | 0 | |
| id_origen | CHAR(36) | SI | | FK → catalogo_origen_reserva |
| id_estado | CHAR(36) | SI | | FK → catalogo_estados |
| referencia_externa | VARCHAR(100) | SI | | |
| tarifa_noche | DECIMAL(12,2) | NO | | |
| noches | INT | NO | | |
| subtotal_hospedaje | DECIMAL(14,2) | NO | | |
| subtotal_servicios | DECIMAL(14,2) | SI | 0 | |
| subtotal_productos | DECIMAL(14,2) | SI | 0 | |
| total_impuestos | DECIMAL(14,2) | SI | 0 | |
| total_descuentos | DECIMAL(14,2) | SI | 0 | |
| total | DECIMAL(14,2) | NO | | |
| total_pagado | DECIMAL(14,2) | SI | 0 | |
| saldo_pendiente | DECIMAL(14,2) | NO | | |
| codigo_descuento | VARCHAR(50) | SI | | |
| porcentaje_descuento | DECIMAL(5,2) | SI | 0 | |
| solicitudes_especiales | TEXT | SI | | |
| notas_internas | TEXT | SI | | |
| checkin_realizado | BOOLEAN | SI | FALSE | |
| checkout_realizado | BOOLEAN | SI | FALSE | |
| fecha_checkin_real | DATETIME | SI | | |
| fecha_checkout_real | DATETIME | SI | | |
| usuario_checkin | CHAR(36) | SI | | FK → usuarios |
| usuario_checkout | CHAR(36) | SI | | FK → usuarios |
| cancelada | BOOLEAN | SI | FALSE | |
| fecha_cancelacion | DATETIME | SI | | |
| motivo_cancelacion | TEXT | SI | | |
| penalizacion_cancelacion | DECIMAL(12,2) | SI | 0 | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

**Índices únicos:** `(id_hotel, numero_reserva)`

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_cliente` → `clientes.id_cliente`
- `id_habitacion` → `habitaciones.id_habitacion`
- `id_tipo_habitacion` → `tipos_habitacion.id_tipo`
- `id_origen` → `catalogo_origen_reserva.id_origen`
- `id_estado` → `catalogo_estados.id_estado`
- `usuario_checkin` → `usuarios.id_usuario`
- `usuario_checkout` → `usuarios.id_usuario`
- `created_by` → `usuarios.id_usuario`

---

### 15. huespedes_reserva
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id | CHAR(36) | NO | UUID() | PK |
| id_reserva | CHAR(36) | NO | | FK → reservas |
| id_cliente | CHAR(36) | SI | | FK → clientes |
| nombre | VARCHAR(100) | NO | | |
| apellido_paterno | VARCHAR(100) | SI | | |
| apellido_materno | VARCHAR(100) | SI | | |
| tipo_documento | CHAR(36) | SI | | FK → catalogo_tipos_documento |
| numero_documento | VARCHAR(50) | SI | | |
| nacionalidad | VARCHAR(100) | SI | | |
| fecha_nacimiento | DATE | SI | | |
| genero | ENUM | SI | NULL | M,F,Otro |
| email | VARCHAR(100) | SI | | |
| telefono | VARCHAR(20) | SI | | |
| es_titular | BOOLEAN | SI | FALSE | |
| es_menor | BOOLEAN | SI | FALSE | |
| parentesco | VARCHAR(50) | SI | | |
| registro_completado | BOOLEAN | SI | FALSE | |
| fecha_registro | DATETIME | SI | | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_reserva` → `reservas.id_reserva`
- `id_cliente` → `clientes.id_cliente`
- `tipo_documento` → `catalogo_tipos_documento.id_tipo_doc`

---

### 16. ocupaciones
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_ocupacion | CHAR(36) | NO | UUID() | PK |
| id_reserva | CHAR(36) | NO | | FK → reservas |
| id_habitacion | CHAR(36) | NO | | FK → habitaciones |
| fecha_checkin | DATETIME | NO | | |
| fecha_checkout | DATETIME | SI | | |
| id_estado | CHAR(36) | SI | | FK → catalogo_estados |
| numero_llave | VARCHAR(50) | SI | | |
| codigo_acceso | VARCHAR(50) | SI | | |
| llaves_entregadas | INT | SI | 1 | |
| llaves_devueltas | INT | SI | 0 | |
| deposito_monto | DECIMAL(12,2) | SI | 0 | |
| deposito_metodo | CHAR(36) | SI | | FK → catalogo_metodos_pago |
| deposito_devuelto | BOOLEAN | SI | FALSE | |
| deposito_deduccion | DECIMAL(12,2) | SI | 0 | |
| deposito_deduccion_motivo | TEXT | SI | | |
| notas_checkin | TEXT | SI | | |
| notas_checkout | TEXT | SI | | |
| notas_housekeeping | TEXT | SI | | |
| inspeccion_realizada | BOOLEAN | SI | FALSE | |
| inspeccion_ok | BOOLEAN | SI | TRUE | |
| danos_reportados | TEXT | SI | | |
| cargos_danos | DECIMAL(12,2) | SI | 0 | |
| usuario_checkin | CHAR(36) | SI | | FK → usuarios |
| usuario_checkout | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |
| updated_at | DATETIME | SI | CURRENT_TIMESTAMP ON UPDATE | |

**Relaciones:**
- `id_reserva` → `reservas.id_reserva`
- `id_habitacion` → `habitaciones.id_habitacion`
- `id_estado` → `catalogo_estados.id_estado`
- `deposito_metodo` → `catalogo_metodos_pago.id_metodo`
- `usuario_checkin` → `usuarios.id_usuario`
- `usuario_checkout` → `usuarios.id_usuario`

---

### 17. catalogo_servicios
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_servicio | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_categoria | CHAR(36) | SI | | FK → catalogo_categorias_producto |
| codigo | VARCHAR(20) | SI | | |
| nombre | VARCHAR(100) | NO | | |
| descripcion | TEXT | SI | | |
| precio | DECIMAL(12,2) | NO | | |
| tipo_cobro | ENUM | NO | | Por noche,Por evento,Por dia,Unico,Por persona,Por hora |
| aplica_impuestos | BOOLEAN | SI | TRUE | |
| disponible_reserva | BOOLEAN | SI | TRUE | |
| disponible_walkin | BOOLEAN | SI | TRUE | |
| requiere_confirmacion | BOOLEAN | SI | FALSE | |
| horario_disponible | JSON | SI | | |
| imagen_url | VARCHAR(500) | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_categoria` → `catalogo_categorias_producto.id_categoria`

---

### 18. reserva_servicios
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id | CHAR(36) | NO | UUID() | PK |
| id_reserva | CHAR(36) | NO | | FK → reservas |
| id_servicio | CHAR(36) | NO | | FK → catalogo_servicios |
| cantidad | DECIMAL(10,2) | SI | 1 | |
| precio_unitario | DECIMAL(12,2) | NO | | |
| descuento | DECIMAL(12,2) | SI | 0 | |
| subtotal | DECIMAL(14,2) | NO | | |
| fecha_aplicacion | DATE | SI | | |
| notas | TEXT | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_reserva` → `reservas.id_reserva`
- `id_servicio` → `catalogo_servicios.id_servicio`
- `created_by` → `usuarios.id_usuario`

---

### 19. productos
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_producto | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_categoria | CHAR(36) | SI | | FK → catalogo_categorias_producto |
| codigo | VARCHAR(50) | SI | | |
| codigo_barras | VARCHAR(50) | SI | | |
| nombre | VARCHAR(100) | NO | | |
| descripcion | TEXT | SI | | |
| precio_venta | DECIMAL(12,2) | NO | | |
| costo | DECIMAL(12,2) | SI | 0 | |
| stock_actual | INT | SI | 0 | |
| stock_minimo | INT | SI | 0 | |
| unidad | VARCHAR(20) | SI | 'PZA' | |
| aplica_impuestos | BOOLEAN | SI | TRUE | |
| imagen_url | VARCHAR(500) | SI | | |
| activo | BOOLEAN | SI | TRUE | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_categoria` → `catalogo_categorias_producto.id_categoria`

---

### 20. consumos_habitacion
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id | CHAR(36) | NO | UUID() | PK |
| id_ocupacion | CHAR(36) | NO | | FK → ocupaciones |
| id_producto | CHAR(36) | SI | | FK → productos |
| id_servicio | CHAR(36) | SI | | FK → catalogo_servicios |
| descripcion | VARCHAR(200) | SI | | |
| cantidad | DECIMAL(10,2) | SI | 1 | |
| precio_unitario | DECIMAL(12,2) | NO | | |
| descuento | DECIMAL(12,2) | SI | 0 | |
| subtotal | DECIMAL(14,2) | NO | | |
| ubicacion | ENUM | SI | 'Habitacion' | Habitacion,Restaurante,Bar,Spa,Tienda,Otro |
| fecha | DATETIME | SI | CURRENT_TIMESTAMP | |
| firmado_por | VARCHAR(100) | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |

**Relaciones:**
- `id_ocupacion` → `ocupaciones.id_ocupacion`
- `id_producto` → `productos.id_producto`
- `id_servicio` → `catalogo_servicios.id_servicio`
- `created_by` → `usuarios.id_usuario`

---

### 21. cargos_reserva
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_cargo | CHAR(36) | NO | UUID() | PK |
| id_reserva | CHAR(36) | NO | | FK → reservas |
| tipo | ENUM | NO | | Hospedaje,Servicio,Producto,Impuesto,Ajuste,Descuento,Penalizacion,Dano |
| referencia_id | CHAR(36) | SI | | |
| referencia_tabla | VARCHAR(50) | SI | | |
| descripcion | VARCHAR(200) | NO | | |
| cantidad | DECIMAL(10,2) | SI | 1 | |
| precio_unitario | DECIMAL(12,2) | NO | | |
| descuento | DECIMAL(12,2) | SI | 0 | |
| subtotal | DECIMAL(14,2) | NO | | |
| es_impuesto | BOOLEAN | SI | FALSE | |
| id_impuesto | CHAR(36) | SI | | FK → impuestos |
| fecha | DATE | NO | | |
| notas | TEXT | SI | | |
| anulado | BOOLEAN | SI | FALSE | |
| motivo_anulacion | TEXT | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_reserva` → `reservas.id_reserva`
- `id_impuesto` → `impuestos.id_impuesto`
- `created_by` → `usuarios.id_usuario`

---

### 22. pagos
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_pago | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_reserva | CHAR(36) | SI | | FK → reservas |
| id_ocupacion | CHAR(36) | SI | | FK → ocupaciones |
| numero_pago | VARCHAR(20) | SI | | |
| fecha | DATETIME | SI | CURRENT_TIMESTAMP | |
| monto | DECIMAL(14,2) | NO | | |
| id_metodo | CHAR(36) | NO | | FK → catalogo_metodos_pago |
| referencia | VARCHAR(100) | SI | | |
| autorizacion | VARCHAR(50) | SI | | |
| ultimos_digitos | VARCHAR(4) | SI | | |
| tipo | ENUM | NO | | Anticipo,Abono,Liquidacion,Reembolso,Deposito |
| comision | DECIMAL(12,2) | SI | 0 | |
| monto_neto | DECIMAL(14,2) | NO | | |
| requiere_factura | BOOLEAN | SI | FALSE | |
| facturado | BOOLEAN | SI | FALSE | |
| uuid_factura | VARCHAR(50) | SI | | |
| notas | TEXT | SI | | |
| anulado | BOOLEAN | SI | FALSE | |
| fecha_anulacion | DATETIME | SI | | |
| motivo_anulacion | TEXT | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_reserva` → `reservas.id_reserva`
- `id_ocupacion` → `ocupaciones.id_ocupacion`
- `id_metodo` → `catalogo_metodos_pago.id_metodo`
- `created_by` → `usuarios.id_usuario`

---

### 23. cambios_reserva
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_cambio | CHAR(36) | NO | UUID() | PK |
| id_reserva | CHAR(36) | NO | | FK → reservas |
| tipo_cambio | ENUM | NO | | Cambio habitacion,Extension noches,Reduccion noches,Cambio fechas,Cambio tarifa,Cambio ocupantes,Cambio estado,Upgrade,Downgrade,Otro |
| campo_modificado | VARCHAR(50) | SI | | |
| valor_anterior | TEXT | SI | | |
| valor_nuevo | TEXT | SI | | |
| impacto_economico | DECIMAL(12,2) | SI | 0 | |
| motivo | TEXT | SI | | |
| id_usuario | CHAR(36) | SI | | FK → usuarios |
| fecha | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_reserva` → `reservas.id_reserva`
- `id_usuario` → `usuarios.id_usuario`

---

### 24. tareas_limpieza
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_tarea | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_habitacion | CHAR(36) | NO | | FK → habitaciones |
| fecha | DATE | NO | | |
| tipo | ENUM | NO | | Checkout,Ocupada,Profunda,Inspeccion |
| prioridad | ENUM | SI | 'Normal' | Baja,Normal,Alta,Urgente |
| id_estado | CHAR(36) | SI | | FK → catalogo_estados |
| asignado_a | CHAR(36) | SI | | FK → usuarios |
| hora_inicio | DATETIME | SI | | |
| hora_fin | DATETIME | SI | | |
| duracion_minutos | INT | SI | | |
| checklist | JSON | SI | | |
| notas | TEXT | SI | | |
| fotos_antes | JSON | SI | | |
| fotos_despues | JSON | SI | | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_habitacion` → `habitaciones.id_habitacion`
- `id_estado` → `catalogo_estados.id_estado`
- `asignado_a` → `usuarios.id_usuario`

---

### 25. ordenes_mantenimiento
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_orden | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_habitacion | CHAR(36) | SI | | FK → habitaciones |
| numero_orden | VARCHAR(20) | SI | | |
| tipo | ENUM | NO | | Preventivo,Correctivo,Emergencia,Mejora |
| categoria | VARCHAR(50) | SI | | |
| descripcion | TEXT | NO | | |
| prioridad | ENUM | SI | 'Normal' | Baja,Normal,Alta,Critica |
| id_estado | CHAR(36) | SI | | FK → catalogo_estados |
| asignado_a | CHAR(36) | SI | | FK → usuarios |
| fecha_reporte | DATETIME | SI | CURRENT_TIMESTAMP | |
| fecha_programada | DATE | SI | | |
| fecha_inicio | DATETIME | SI | | |
| fecha_fin | DATETIME | SI | | |
| costo_estimado | DECIMAL(12,2) | SI | | |
| costo_real | DECIMAL(12,2) | SI | | |
| materiales_usados | JSON | SI | | |
| notas | TEXT | SI | | |
| fotos | JSON | SI | | |
| reportado_por | CHAR(36) | SI | | FK → usuarios |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_habitacion` → `habitaciones.id_habitacion`
- `id_estado` → `catalogo_estados.id_estado`
- `asignado_a` → `usuarios.id_usuario`
- `reportado_por` → `usuarios.id_usuario`

---

### 26. bloqueos_habitacion
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_bloqueo | CHAR(36) | NO | UUID() | PK |
| id_habitacion | CHAR(36) | NO | | FK → habitaciones |
| fecha_inicio | DATE | NO | | |
| fecha_fin | DATE | NO | | |
| motivo | ENUM | NO | | Mantenimiento,Remodelacion,Reserva interna,Fuera de servicio,Otro |
| descripcion | TEXT | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_habitacion` → `habitaciones.id_habitacion`
- `created_by` → `usuarios.id_usuario`

---

### 27. bitacora_recepcion
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| turno | ENUM | NO | | Matutino,Vespertino,Nocturno |
| fecha | DATE | NO | | |
| hora_inicio | DATETIME | SI | | |
| hora_fin | DATETIME | SI | | |
| usuario_turno | CHAR(36) | SI | | FK → usuarios |
| habitaciones_ocupadas | INT | SI | 0 | |
| habitaciones_disponibles | INT | SI | 0 | |
| checkins_esperados | INT | SI | 0 | |
| checkins_realizados | INT | SI | 0 | |
| checkouts_esperados | INT | SI | 0 | |
| checkouts_realizados | INT | SI | 0 | |
| fondo_inicial | DECIMAL(12,2) | SI | 0 | |
| total_efectivo | DECIMAL(12,2) | SI | 0 | |
| total_tarjeta | DECIMAL(12,2) | SI | 0 | |
| total_transferencia | DECIMAL(12,2) | SI | 0 | |
| fondo_final | DECIMAL(12,2) | SI | 0 | |
| diferencia | DECIMAL(12,2) | SI | 0 | |
| novedades | TEXT | SI | | |
| pendientes | TEXT | SI | | |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `usuario_turno` → `usuarios.id_usuario`

---

### 28. facturas
| Columna | Tipo | Nulo | Default | Descripción |
|---------|------|------|---------|-------------|
| id_factura | CHAR(36) | NO | UUID() | PK |
| id_hotel | CHAR(36) | NO | | FK → hoteles |
| id_reserva | CHAR(36) | SI | | FK → reservas |
| id_cliente | CHAR(36) | NO | | FK → clientes |
| serie | VARCHAR(10) | SI | | |
| folio | VARCHAR(20) | NO | | |
| uuid_sat | VARCHAR(50) | SI | | |
| fecha_emision | DATETIME | SI | CURRENT_TIMESTAMP | |
| fecha_timbrado | DATETIME | SI | | |
| rfc_emisor | VARCHAR(20) | SI | | |
| rfc_receptor | VARCHAR(20) | SI | | |
| razon_social_receptor | VARCHAR(200) | SI | | |
| uso_cfdi | VARCHAR(10) | SI | | |
| regimen_fiscal | VARCHAR(10) | SI | | |
| metodo_pago | VARCHAR(10) | SI | | |
| forma_pago | VARCHAR(10) | SI | | |
| subtotal | DECIMAL(14,2) | NO | | |
| total_impuestos | DECIMAL(14,2) | SI | 0 | |
| total | DECIMAL(14,2) | NO | | |
| xml_cfdi | LONGTEXT | SI | | |
| pdf_url | VARCHAR(500) | SI | | |
| estado | ENUM | SI | 'Emitida' | Emitida,Timbrada,Cancelada,Error |
| motivo_cancelacion | TEXT | SI | | |
| fecha_cancelacion | DATETIME | SI | | |
| created_by | CHAR(36) | SI | | FK → usuarios |
| created_at | DATETIME | SI | CURRENT_TIMESTAMP | |

**Relaciones:**
- `id_hotel` → `hoteles.id_hotel`
- `id_reserva` → `reservas.id_reserva`
- `id_cliente` → `clientes.id_cliente`
- `created_by` → `usuarios.id_usuario`

---

## Diagrama de Relaciones

```
hoteles (CENTRAL)
├── usuarios
├── catalogo_estados
├── catalogo_metodos_pago
├── catalogo_origen_reserva
├── catalogo_categorias_producto
├── impuestos
├── tipos_habitacion
│   ├── tarifas_habitacion
│   └── habitaciones
│       ├── habitacion_inventario
│       ├── bloqueos_habitacion
│       ├── tareas_limpieza
│       ├── ordenes_mantenimiento
│       └── ocupaciones
│           └── consumos_habitacion
├── clientes
│   ├── reservas
│   │   ├── huespedes_reserva
│   │   ├── reserva_servicios
│   │   ├── cargos_reserva
│   │   ├── cambios_reserva
│   │   ├── pagos
│   │   └── ocupaciones
│   └── facturas
├── catalogo_servicios
├── productos
├── bitacora_recepcion
└── facturas
```

---

## Índices de Rendimiento

| Tabla | Índice | Columnas |
|-------|--------|----------|
| reservas | idx_reservas_fechas | fecha_checkin, fecha_checkout |
| reservas | idx_reservas_estado | id_estado |
| reservas | idx_reservas_cliente | id_cliente |
| ocupaciones | idx_ocupaciones_fechas | fecha_checkin, fecha_checkout |
| habitaciones | idx_habitaciones_estado | estado_habitacion, estado_limpieza |
| pagos | idx_pagos_reserva | id_reserva |
| cargos_reserva | idx_cargos_reserva | id_reserva |
| tareas_limpieza | idx_tareas_fecha | fecha, id_estado |
| clientes | idx_clientes_email | email |
| clientes | idx_clientes_telefono | telefono |
