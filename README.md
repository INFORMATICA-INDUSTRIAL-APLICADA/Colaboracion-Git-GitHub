# Colaboración, Ramas y Resolución de Conflictos

En esta práctica vamos a subir de nivel. Ya no trabajarás solo/a: vas a colaborar en el código de un compañero/a. Aprenderás qué pasa cuando dos personas tocan el mismo archivo y cómo Git nos ayuda a solucionar _choques_ de código.

## 0. ¿Qué es una rama y para qué sirve?

Una **rama** (*branch*) es una línea de trabajo independiente dentro de un repositorio. Al crear una rama desde `main`, partes de la misma versión del código, pero los cambios que hagas en ella no afectan a la rama principal hasta que decidas integrarlos.

Su objetivo es poder desarrollar una funcionalidad, corregir un error o probar una idea de forma aislada y segura. Así, varias personas pueden trabajar al mismo tiempo en tareas diferentes sin sobrescribirse los cambios. Cuando el trabajo está listo, la rama se fusiona con `main`, normalmente mediante un Pull Request.

## 1. ¿Qué es un Pull Request?

Un **Pull Request (PR)** es una funcionalidad de plataformas como **GitHub**, no de Git. Git permite crear ramas y fusionarlas; GitHub ofrece el PR como una propuesta para incorporar los cambios de una rama a otra, normalmente a la rama principal (`main`). No modifica el código automáticamente: permite ver qué archivos han cambiado, comentar líneas concretas, revisar el trabajo y comprobar que todo funciona antes de fusionarlo.

En proyectos reales, los PR ayudan a mantener la calidad del código y a evitar que cambios incompletos o incompatibles lleguen a producción. Es habitual que otro miembro del equipo revise y apruebe el PR antes de integrarlo. En esta práctica usaréis un PR para compartir el trabajo entre ambos repositorios y aprender a resolver el conflicto que aparece cuando dos ramas modifican las mismas líneas.

La fusión la realiza una persona con permiso para modificar la rama de destino: por ejemplo, el responsable técnico, un miembro del equipo propietario del código o un mantenedor del proyecto. En las empresas suelen existir reglas que exigen una o varias aprobaciones y que las pruebas automáticas sean correctas antes de permitir la fusión. En los proyectos de software libre, los colaboradores proponen cambios mediante PR y los mantenedores deciden si los aceptan, solicitan modificaciones o los cierran, siguiendo las normas y objetivos del proyecto.

## 2. Preparación de la Pareja

1.  **Roles recíprocos:** Cada miembro de la pareja será **Propietario** de su propio repositorio y **Colaborador** en el repositorio de su compañero/a.
2.  **Invitación:**
    *   Cada **Propietario** debe ir a su repositorio de GitHub, abrir **Settings > Collaborators > Add people** e invitar a su compañero/a.
    *   Cada **Colaborador** debe aceptar la invitación recibida por correo o en las notificaciones de GitHub.
3.  **Clonado:** Cada miembro debe clonar en VS Code el repositorio de su compañero/a, en el que trabajará como **Colaborador**.


## 3. El Código Base

El **Propietario** debe crear el archivo `tareas.py`, copiar este código, hacer commit y subirlo (`push`) a la rama `main`:

```python
# tareas.py - Gestor de Tareas Simple

tareas = ["Comprar leche", "Estudiar Git", "Lavar el coche"]

def mostrar_tareas():
    print("\n--- LISTA DE TAREAS ---")
    for i, tarea in enumerate(tareas):
        print(f"{i + 1}. {tarea}")

if __name__ == "__main__":
    mostrar_tareas()
```

*Una vez subido, el **Colaborador** debe hacer un `pull` o clonar para tener este mismo archivo.*

---

## 4. Trabajo en Ramas (Branching)

Para no estropear la rama principal (`main`), cada uno trabajará en una rama distinta.

### Alumno A (Propietario) -> Rama: `feat-añadir`
1.  Crea la rama `feat-añadir` en VS Code.
2.  Añade la función para añadir tareas y modficia el final del archivo:
```python
def añadir_tarea(nueva_tarea):
    tareas.append(nueva_tarea)

# ... (en el bloque final)
if __name__ == "__main__":
    añadir_tarea("Ir al gimnasio")
    mostrar_tareas()
```
3.  Haz commit y sube la rama (`Publish Branch`).

### Alumno B (Colaborador) -> Rama: `feat-contar`
1.  Crea la rama `feat-contar` en VS Code.
2.  Añade la función para contar tareas y modifica el final del archivo:
```python
def contar_tareas():
    print(f"Total: {len(tareas)} tareas.")

# ... (en el bloque final)
if __name__ == "__main__":
    mostrar_tareas()
    contar_tareas()
```
3.  Haz commit y sube la rama (`Publish Branch`).

---

## 5. La Primera Integración (Sin problemas)

El **Propietario** va a fusionar su propia rama primero:
1.  En VS Code, vuelve a la rama `main`.
2.  Haz clic en el menú de tres puntos `...` de la pestaña de Git -> **Branch** -> **Merge Branch...** y elige `feat-añadir`.
3.  Sincroniza los cambios (**Push**). Ahora `main` en GitHub ya tiene la función de añadir tareas.

---

## 6. El Conflicto (El momento de la verdad)

Ahora el **Colaborador** intentará enviar su trabajo.
1.  **Colaborador:** Ve a GitHub y abre un **Pull Request (PR)** desde tu rama `feat-contar` hacia la `main` del propietario.
2.  **Propietario:** Verás que GitHub te da un aviso en rojo: *"This branch has conflicts that must be resolved"*.

**¿Por qué ha pasado esto?** Porque ambos habéis modificado las líneas del `if __name__ == "__main__":`. Git no sabe cuál de las dos versiones es la correcta y pide ayuda humana.

**¿Qué ocurre si el colaborador modifica algo que ya ha cambiado el propietario u otro colaborador?** En un equipo con tareas repartidas, puede indicar falta de coordinación: quizá dos personas han trabajado en la misma parte sin acordarlo o alguien ha cambiado una zona que no tenía asignada. Por eso conviene dividir las tareas, comunicar qué se está haciendo y revisar los cambios antes de integrarlos.

En un proyecto de software libre esta situación también es habitual y no implica necesariamente una mala organización. Los colaboradores pueden trabajar por iniciativa propia y proponer mejoras sobre partes del código que ya han cambiado otros. En ese caso, los mantenedores revisan el PR, deciden qué propuesta encaja mejor con el proyecto y, si es necesario, piden adaptar los cambios antes de fusionarlos.

---

## 7. Resolviendo el Conflicto desde VS Code

El **Propietario** resolverá el conflicto siguiendo estos pasos:

1.  En VS Code, asegúrate de estar en `main` y pulsa **Sincronizar cambios** (el icono de flechas circulares) para actualizar la información del repositorio. Si no aparece la rama del compañero, abre el menú de tres puntos `...` de la pestaña de Git y elige **Fetch**.
2.  Desde el menú de tres puntos `...`, selecciona **Branch** -> **Merge Branch...** y elige `origin/feat-contar`.
3.  **¡Pánico!** El archivo `tareas.py` se volverá loco y mostrará algo así:

```python
<<<<<<< HEAD (Current Change: Lo que ya hay en main)
    añadir_tarea("Ir al gimnasio")
    mostrar_tareas()
=======
    mostrar_tareas()
    contar_tareas()
>>>>>>> feat-contar (Incoming Change: Lo que trae el compañero)
```

4.  **La solución fácil:** Encima de ese bloque, VS Code te ofrece opciones en letras pequeñas. Haz clic en **"Accept Both Changes"** (Aceptar ambos cambios).
5.  **Limpieza:** Borra las líneas repetidas de `mostrar_tareas()` si es necesario para que el código quede limpio y funcional:
```python
if __name__ == "__main__":
    añadir_tarea("Ir al gimnasio")
    mostrar_tareas()
    contar_tareas()
```
6.  **Finalizar:** Guarda el archivo, haz un commit (ej: `fix: Resuelve conflicto de integración`) y pulsa **Sincronizar**.

---

## 8. Resultado Final

*   **Propietario:** Tu rama `main` ahora tiene el código de ambos perfectamente integrado.
*   **Colaborador:** Haz `git checkout main` y luego `git pull`. ¡Ya tienes el trabajo de los dos en tu ordenador!

**Entrega:** El repositorio del Propietario debe mostrar el historial de ramas, el Pull Request cerrado y el archivo `tareas.py` final con todas las funciones.