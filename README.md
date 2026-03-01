#include <stdio.h>
#include <string.h>

#define PARCIALES 3
#define MATERIAS 4
#define ALUMNOS 5
#define APROBACION 6.0

int main() {

    /* ===== VARIABLES ===== */
    float calificaciones[PARCIALES][MATERIAS][ALUMNOS]; // Arreglo 3D obligatorio
    char nombresAlumnos[ALUMNOS][30];   // Arreglo 1D auxiliar (2D para strings)
    char nombresMaterias[MATERIAS][30];
    char estatus;
    
    int i, j, k;
    int opcion;
    int parcial, materia, alumno;
    float suma, promedio;
    float max, min;
    int posMateriaMax, posAlumnoMax;
    int posMateriaMin, posAlumnoMin;

    /* ===== INICIALIZAR ARREGLO EN 0 ===== */
    for(i = 0; i < PARCIALES; i++)
        for(j = 0; j < MATERIAS; j++)
            for(k = 0; k < ALUMNOS; k++)
                calificaciones[i][j][k] = 0.0;

    /* ===== CAPTURAR NOMBRES ===== */
    printf("=== CAPTURA DE MATERIAS ===\n");
    for(i = 0; i < MATERIAS; i++){
        printf("Nombre de la materia %d: ", i+1);
        scanf("%s", nombresMaterias[i]);
    }

    printf("\n=== CAPTURA DE ALUMNOS ===\n");
    for(i = 0; i < ALUMNOS; i++){
        printf("Nombre del alumno %d: ", i+1);
        scanf("%s", nombresAlumnos[i]);
    }

    /* ===== MENU PRINCIPAL ===== */
    do {
        printf("\n===== MENU =====\n");
        printf("1. Capturar calificaciones\n");
        printf("2. Mostrar calificaciones\n");
        printf("3. Modificar calificacion\n");
        printf("4. Promedios\n");
        printf("5. Maximo y minimo por parcial\n");
        printf("6. Salir\n");
        printf("Seleccione opcion: ");
        scanf("%d", &opcion);

        switch(opcion){

        case 1:
            for(i = 0; i < PARCIALES; i++){
                printf("\nPARCIAL %d\n", i+1);
                for(j = 0; j < MATERIAS; j++){
                    for(k = 0; k < ALUMNOS; k++){

                        do{
                            printf("Calificacion P%d - %s - %s (0-10): ",
                                   i+1, nombresMaterias[j], nombresAlumnos[k]);
                            scanf("%f", &calificaciones[i][j][k]);

                        }while(calificaciones[i][j][k] < 0.0 || 
                               calificaciones[i][j][k] > 10.0);
                    }
                }
            }
            break;

        case 2:
            for(i = 0; i < PARCIALES; i++){
                printf("\n=== PARCIAL %d ===\n", i+1);
                for(j = 0; j < MATERIAS; j++){
                    printf("\nMateria: %s\n", nombresMaterias[j]);
                    for(k = 0; k < ALUMNOS; k++){

                        if(calificaciones[i][j][k] >= APROBACION)
                            estatus = 'A';
                        else
                            estatus = 'R';

                        printf("%s: %.2f (%c)\n",
                               nombresAlumnos[k],
                               calificaciones[i][j][k],
                               estatus);
                    }
                }
            }
            break;

        case 3:
            printf("Parcial (1-3): ");
            scanf("%d", &parcial);
            printf("Materia (1-4): ");
            scanf("%d", &materia);
            printf("Alumno (1-5): ");
            scanf("%d", &alumno);

            while(parcial < 1 || parcial > PARCIALES ||
                  materia < 1 || materia > MATERIAS ||
                  alumno < 1 || alumno > ALUMNOS){
                printf("Indice fuera de rango. Intente de nuevo.\n");
                printf("Parcial (1-3): "); scanf("%d", &parcial);
                printf("Materia (1-4): "); scanf("%d", &materia);
                printf("Alumno (1-5): "); scanf("%d", &alumno);
            }

            printf("Nueva calificacion: ");
            scanf("%f", &calificaciones[parcial-1][materia-1][alumno-1]);
            break;

        case 4:
            printf("\n=== PROMEDIO POR ALUMNO ===\n");
            for(k = 0; k < ALUMNOS; k++){
                suma = 0;
                for(i = 0; i < PARCIALES; i++)
                    for(j = 0; j < MATERIAS; j++)
                        suma += calificaciones[i][j][k];

                promedio = suma / (PARCIALES * MATERIAS);
                printf("%s: %.2f\n", nombresAlumnos[k], promedio);
            }

            printf("\n=== PROMEDIO POR MATERIA (POR PARCIAL) ===\n");
            for(i = 0; i < PARCIALES; i++){
                printf("Parcial %d\n", i+1);
                for(j = 0; j < MATERIAS; j++){
                    suma = 0;
                    for(k = 0; k < ALUMNOS; k++)
                        suma += calificaciones[i][j][k];

                    promedio = suma / ALUMNOS;
                    printf("%s: %.2f\n", nombresMaterias[j], promedio);
                }
            }
            break;

        case 5:
            printf("\n=== MAXIMO Y MINIMO POR PARCIAL ===\n");
            for(i = 0; i < PARCIALES; i++){

                max = calificaciones[i][0][0];
                min = calificaciones[i][0][0];

                for(j = 0; j < MATERIAS; j++){
                    for(k = 0; k < ALUMNOS; k++){

                        if(calificaciones[i][j][k] > max){
                            max = calificaciones[i][j][k];
                            posMateriaMax = j;
                            posAlumnoMax = k;
                        }

                        if(calificaciones[i][j][k] < min){
                            min = calificaciones[i][j][k];
                            posMateriaMin = j;
                            posAlumnoMin = k;
                        }
                    }
                }

                printf("Parcial %d\n", i+1);
                printf("Maximo: %.2f (%s - %s)\n",
                       max,
                       nombresMaterias[posMateriaMax],
                       nombresAlumnos[posAlumnoMax]);

                printf("Minimo: %.2f (%s - %s)\n",
                       min,
                       nombresMaterias[posMateriaMin],
                       nombresAlumnos[posAlumnoMin]);
            }
            break;

        case 6:
            printf("Saliendo del sistema...\n");
            break;

        default:
            printf("Opcion invalida\n");
        }

    }while(opcion != 6);

    return 0;
}
