PAV - P3: detección de pitch
============================

Esta práctica se distribuye a través del repositorio GitHub [Práctica 3](https://github.com/albino-pav/P3).
Siga las instrucciones de la [Práctica 2](https://github.com/albino-pav/P2) para realizar un `fork` de la
misma y distribuir copias locales (*clones*) del mismo a los distintos integrantes del grupo de prácticas.

Recuerde realizar el *pull request* al repositorio original una vez completada la práctica.

Ejercicios básicos
------------------

- Complete el código de los ficheros necesarios para realizar la detección de pitch usando el programa
  `get_pitch`.

   * Complete el cálculo de la autocorrelación e inserte a continuación el código correspondiente.
   
   ```
   for (unsigned int l = 0; l < r.size(); ++l) {
  	/// Compute the autocorrelation r[l]
	r[l] = 0;
    	for(unsigned int k = l; k < x.size();k++){
        	r[l] += x[k] * x[k - l];
      	}
      	r[l] /= x.size();
   }
   ```

   * Inserte una gŕafica donde, en un *subplot*, se vea con claridad la señal temporal de un sonido sonoro
     y su periodo de pitch; y, en otro *subplot*, se vea con claridad la autocorrelación de la señal y la
	 posición del primer máximo secundario.

	 NOTA: es más que probable que tenga que usar Python, Octave/MATLAB u otro programa semejante para
	 hacerlo. Se valorará la utilización de la librería matplotlib de Python.
	 
		<img src="img/SubplotCode.jpeg" width="640" align="center">
		<img src="img/Subplot.jpeg" width="640" align="center">

   * Determine el mejor candidato para el periodo de pitch localizando el primer máximo secundario de la
     autocorrelación. Inserte a continuación el código correspondiente.
     
   ```  
   for(iRactual = iR + npitch_min; iRactual < iR + npitch_max; iRactual++) {
      if(*iRactual > *iRMax) {
        iRMax = iRactual;
      }
    }
    unsigned int lag = 0;

    if (iRMax != r.end())
      lag = iRMax - r.begin();

    float pot = 10 * log10(r[0]);

    ```

   * Implemente la regla de decisión sonoro o sordo e inserte el código correspondiente.
   ```
   if(pot <= 0 && r1norm<0.8839F)
      return true;
   else
      return false;
   ```
- Una vez completados los puntos anteriores, dispondrá de una primera versión del detector de pitch. El 
  resto del trabajo consiste, básicamente, en obtener las mejores prestaciones posibles con él.

  * Utilice el programa `wavesurfer` para analizar las condiciones apropiadas para determinar si un
    segmento es sonoro o sordo. 
	
	  - Inserte una gráfica con la detección de pitch incorporada a `wavesurfer` y, junto a ella, los 
	    principales candidatos para determinar la sonoridad de la voz: el nivel de potencia de la señal
		(r[0]), la autocorrelación normalizada de uno (r1norm = r[1] / r[0]) y el valor de la
		autocorrelación en su máximo secundario (rmaxnorm = r[lag] / r[0]).

		Puede considerar, también, la conveniencia de usar la tasa de cruces por cero.

	    Recuerde configurar los paneles de datos para que el desplazamiento de ventana sea el adecuado, que
		en esta práctica es de 15 ms.

      - Use el detector de pitch implementado en el programa `wavesurfer` en una señal de prueba y compare
	    su resultado con el obtenido por la mejor versión de su propio sistema.  Inserte una gráfica
		ilustrativa del resultado de ambos detectores.
		
		<img src="img/Vers1.jpeg" width="640" align="center">
  
  * Optimice los parámetros de su sistema de detección de pitch e inserte una tabla con las tasas de error
    y el *score* TOTAL proporcionados por `pitch_evaluate` en la evaluación de la base de datos 
	`pitch_db/train`..
	
   | File | Unvoiced frames as voiced | Voiced frames as unvoiced | Gross voiced errors | MSE of fine errors | TOTAL |
   | ----------------- | ----------------- | ----------------- | ----------------- | ----------------- | ----------------- |
   | rl002| 15.66% | 0.00% | 5.88% | 2.09% | 87.05% |
   | rl004| 15.66% |   $12 | $1600 | $1600 | $1600 |
   | rl006| 15.66% |    $1 | $1600 | $1600 | $1600 |
   | rl008| 15.66% | $1600 | $1600 | $1600 | $1600 |
   | rl010| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl012| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl014| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl016| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl018| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl020| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl022| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl024| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl026| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl028| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl030| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl032| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl034| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl036| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl038| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl040| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl042| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl044| right-aligned | $1600 | $1600 | $1600 | $1600 |
   | rl046| centered      |   $12 | $1600 | $1600 | $1600 |
   | rl048| are neat      |    $1 | $1600 | $1600 | $1600 |
   | rl050| right-aligned | $1600 | $1600 | $1600 | $1600 |

   * Inserte una gráfica en la que se vea con claridad el resultado de su detector de pitch junto al del
     detector de Wavesurfer. Aunque puede usarse Wavesurfer para obtener la representación, se valorará
	 el uso de alternativas de mayor calidad (particularmente Python).
	 
		<img src="img/Pitch.jpeg" width="640" align="center">
   

Ejercicios de ampliación
------------------------

- Usando la librería `docopt_cpp`, modifique el fichero `get_pitch.cpp` para incorporar los parámetros del
  detector a los argumentos de la línea de comandos.
  
  Esta técnica le resultará especialmente útil para optimizar los parámetros del detector. Recuerde que
  una parte importante de la evaluación recaerá en el resultado obtenido en la detección de pitch en la
  base de datos.

  * Inserte un *pantallazo* en el que se vea el mensaje de ayuda del programa y un ejemplo de utilización
    con los argumentos añadidos.

- Implemente las técnicas que considere oportunas para optimizar las prestaciones del sistema de detección
  de pitch.

  Entre las posibles mejoras, puede escoger una o más de las siguientes:

  * Técnicas de preprocesado: filtrado paso bajo, *center clipping*, etc.
  * Técnicas de postprocesado: filtro de mediana, *dynamic time warping*, etc.
  * Métodos alternativos a la autocorrelación: procesado cepstral, *average magnitude difference function*
    (AMDF), etc.
  * Optimización **demostrable** de los parámetros que gobiernan el detector, en concreto, de los que
    gobiernan la decisión sonoro/sordo.
  * Cualquier otra técnica que se le pueda ocurrir o encuentre en la literatura.

  Encontrará más información acerca de estas técnicas en las [Transparencias del Curso](https://atenea.upc.edu/pluginfile.php/2908770/mod_resource/content/3/2b_PS Techniques.pdf)
  y en [Spoken Language Processing](https://discovery.upc.edu/iii/encore/record/C__Rb1233593?lang=cat).
  También encontrará más información en los anexos del enunciado de esta práctica.

  Incluya, a continuación, una explicación de las técnicas incorporadas al detector. Se valorará la
  inclusión de gráficas, tablas, código o cualquier otra cosa que ayude a comprender el trabajo realizado.

  También se valorará la realización de un estudio de los parámetros involucrados. Por ejemplo, si se opta
  por implementar el filtro de mediana, se valorará el análisis de los resultados obtenidos en función de
  la longitud del filtro.
   

Evaluación *ciega* del detector
-------------------------------

Antes de realizar el *pull request* debe asegurarse de que su repositorio contiene los ficheros necesarios
para compilar los programas correctamente ejecutando `make release`.

Con los ejecutables construidos de esta manera, los profesores de la asignatura procederán a evaluar el
detector con la parte de test de la base de datos (desconocida para los alumnos). Una parte importante de
la nota de la práctica recaerá en el resultado de esta evaluación.
