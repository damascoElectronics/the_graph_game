# GitHub Graph Builder Automático

Sistema automatizado para generar commits en GitHub de lunes a viernes entre 18:00-22:00.

## Instalación Rápida

### 1. Preparar repositorio GitHub
```bash
# Crear o clonar tu repositorio
git clone https://github.com/tu-usuario/tu-repo.git
cd tu-repo

# O crear uno nuevo
mkdir github-graph-repo
cd github-graph-repo
git init
git remote add origin https://github.com/tu-usuario/tu-repo.git
```

### 2. Instalar el sistema
```bash
chmod +x install.sh
./install.sh
```

El script te pedirá:
- Ruta del repositorio
- Frecuencia de ejecución (cada hora, 30min, o 15min)

### 3. Verificar instalación
```bash
# Ver crontab configurado
crontab -l

# Ver logs
tail -f /tmp/github-graph.log
```

## Instalación Manual

### 1. Copiar script
```bash
cp github-graph-auto.sh ~/github-graph-auto.sh
chmod +x ~/github-graph-auto.sh
```

### 2. Editar ruta del repositorio
Abre el script y cambia esta línea:
```bash
REPO_DIR="$HOME/github-graph-repo"  # Tu ruta aquí
```

### 3. Configurar crontab
```bash
crontab -e
```

Añade una de estas líneas:

**Opción 1: Cada hora (18:00, 19:00, 20:00, 21:00, 22:00)**
```
0 18-22 * * 1-5 /bin/bash $HOME/github-graph-auto.sh
```

**Opción 2: Cada 30 minutos**
```
*/30 18-22 * * 1-5 /bin/bash $HOME/github-graph-auto.sh
```

**Opción 3: Cada 15 minutos**
```
*/15 18-22 * * 1-5 /bin/bash $HOME/github-graph-auto.sh
```

## Funcionamiento

- **Cuándo:** Lunes a viernes, 18:00-22:00
- **Qué hace:** Genera 1-3 commits aleatorios cada ejecución
- **Logs:** Se guardan en `/tmp/github-graph.log`

## Comandos Útiles

```bash
# Ver estado del cron
crontab -l

# Ver logs en tiempo real
tail -f /tmp/github-graph.log

# Probar manualmente
~/github-graph-auto.sh

# Eliminar del cron
crontab -e
# (borrar la línea del github-graph)

# Ver commits generados
cd ~/github-graph-repo
git log --oneline -20
```

## Personalización

### Cambiar número de commits
Edita esta línea en `github-graph-auto.sh`:
```bash
NUM_COMMITS=$((RANDOM % 3 + 1))  # 1-3 commits
# Cambiar a:
NUM_COMMITS=$((RANDOM % 5 + 1))  # 1-5 commits
```

### Cambiar horario
Modifica el crontab:
```bash
crontab -e
```

Ejemplos:
- `0 20-23 * * 1-5` → 20:00-23:00
- `0 9-17 * * 1-5` → 9:00-17:00 (horario laboral)
- `0 18-22 * * *` → Todos los días (incluye fines de semana)

## Solución de Problemas

### Los commits no aparecen
```bash
# Verificar que el script se ejecuta
tail -f /tmp/github-graph.log

# Verificar cron
crontab -l

# Probar manualmente
cd ~/github-graph-repo
~/github-graph-auto.sh
```

### Error de permisos
```bash
chmod +x ~/github-graph-auto.sh
```

### Error de push
Verifica tu configuración de Git:
```bash
cd ~/github-graph-repo
git config user.name "Tu Nombre"
git config user.email "tu@email.com"

# Para HTTPS con token
git remote set-url origin https://TOKEN@github.com/usuario/repo.git

# Para SSH
git remote set-url origin git@github.com:usuario/repo.git
```

## Desinstalación

```bash
# Remover del crontab
crontab -e
# (borrar la línea)

# Eliminar archivos
rm ~/github-graph-auto.sh
rm /tmp/github-graph.log
```
