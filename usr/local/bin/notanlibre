#!/bin/bash

# Nombre:    CREALIB LIBERSYS - Comando "notanlibre"
# Autor:    Charlie Martínez® <cmartinez@quirinux.org>
# Licencia:    https://www.gnu.org/licenses/old-licenses/gpl-2.0.txt
# Descripción:    Reinstalar componentes privativos desinstalados con el comando "libre"
# Versión:    1.0

FOLDER_NONFREE="/opt/libersys"
FOLDER_UNINSTALLED="$FOLDER_NONFREE/desinstalados"
SELECCION=""

function _menu_lista() {
    clear
    local opciones=()
    for deb_file in "$FOLDER_UNINSTALLED"/*.deb; do
        opciones+=("$(basename "$deb_file")" "" off)
    done

    if [ ${#opciones[@]} -eq 0 ]; then
        dialog --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
            --msgbox "No se detectaron componentes privativos que hayan sido eliminados mediante el uso del comando 'libre'" 10 50
        return 1
    fi

    SELECCION=$(dialog \
        --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
        --separate-output --checklist "Selecciona componentes a reinstalar (BARRA ESPACIADORA)" 20 60 10 "${opciones[@]}" \
        2>&1 >/dev/tty)

    # Si el usuario elige "Cancelar", sale del script
    local dialog_exit_code=$?
    [ $dialog_exit_code -eq 1 ] && return 1

    # Si no se seleccionaron componentes, muestra un mensaje y vuelve a mostrar el menú
    if [ -z "$SELECCION" ]; then
        dialog --msgbox "No se seleccionaron componentes." 20 60
        return 1
    fi

    return 0
}

function _despedida() {
    clear
    echo 'Gracias por utilizar Crealib Libersys - Comando "notanlibre"'
    echo 'Incluido en quirinux-terminal-tools'
    echo '-------------------------------------------------------'
    echo 'Licencia GPLv2 | Autor: Charlie Martínez'
    echo 'Contacto: cmartinez@quirinux.org'
    echo ''
    exit 1
}

# Verifica si el usuario tiene permisos de administrador

if [ "$EUID" -ne 0 ]; then
    echo ''
    echo "Este script debe ejecutarse con permisos de root."
    echo 'Puedes intentarlo con "sudo" tuusuario, "sudo su" o "su root".'
    echo ''
    exit 1
fi

# Verifica si está instalado dialog
if ! command -v dialog &>/dev/null; then
    echo "El programa 'dialog' es necesario para ejecutar este script."
    echo "¿Deseas instalarlo? (1) Aceptar (2) Cancelar"
    read -r choice

    case $choice in
        1)
              apt-get update
              apt-get install -y dialog
            ;;
        *)
            _despedida
            ;;
    esac
fi

# Continúa la ejecución utilizando la interfaz dialog

# Muestra la información sobre la utilidad del programa
clear
dialog --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
    --title 'Comando "notanlibre"' \
    --msgbox '\nEste programa sirve para reinstalar componentes privativos que hayan sido desinstalados con el comando "libre".' 10 50 \

# Verifica si existe FOLDER_UNINSTALLED
if [ ! -d "$FOLDER_UNINSTALLED" ]; then
    dialog --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
        --msgbox "No se detectaron componentes privativos que hayan sido eliminados mediante el uso del comando 'libre'" 10 50
    _despedida
fi

# Verifica si la carpeta FOLDER_UNINSTALLED contiene elementos
if [ -z "$(ls -A "$FOLDER_UNINSTALLED")" ]; then
    dialog --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
        --msgbox "No se detectaron componentes privativos que hayan sido eliminados mediante el uso del comando 'libre'" 10 50
    _despedida
fi

# Muestra la lista y confirma la reinstalación
_menu_lista
dialog_exit_code=$?

# Verifica el código de salida del dialog
if [ $dialog_exit_code -eq 1 ]; then
    _despedida
fi

# Solicita confirmación
clear
dialog --backtitle "Crealib Libersys v1.0 - Comando 'notanlibre' by Charlie Martínez®" \
    --yesno "Se van a reinstalar los siguientes elementos:\n\n$SELECCION" 10 60

# Almacena el código de salida en otra variable
continue_exit_code=$?

# Si el usuario elige Sí, reinstala los paquetes seleccionados. Si elige No, sale del script.
if [ $continue_exit_code -eq 0 ]; then
    # Reinstala los paquetes seleccionados
    clear
    for deb_file in $SELECCION; do
        cd "$FOLDER_UNINSTALLED"
          apt install "./$deb_file" -y
        # Elimina el paquete reinstalado
        rm "$FOLDER_UNINSTALLED/$deb_file"
    done

    # Mensaje de despedida
    clear
    dialog --backtitle 'Crealib Libersys, comando "libre" by Charlie Martínez®' \
        --msgbox "La reinstalación fue exitosa.\nA veces los componentes privativos son necesarios.\nLamentamos que ahora tu sistema sea un poco menos libre que antes :-(" 10 40 \
        
    clear
else
    # Usuario elige No
    _despedida
fi
