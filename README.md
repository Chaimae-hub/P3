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
   | rl004| 41.30% | 1.64% | 5.00% | 3.60% | 74.93% |
   | rl006| 23.08% | 7.25% | 3.12% | 2.36% | 82.18% |
   | rl008| 26.44% | 4.26% | 6.67% | 2.22% | 77.92% |
   | rl010| 22.73% | 20.79% | 3.75% | 3.05% | 74.48% |
   | rl012| 32.86% | 0.00% | 2.27% | 2.71% | 77.22% |
   | rl014| 27.50% | 1.67% | 5.08% | 1.20% | 84.18% |
   | rl016| 22.22% | 13.68% | 9.76% | 3.82% | 75.44% |
   | rl018| 42.86% | 0.00% | 1.69% | 1.60% | 80.10% |
   | rl020| 21.82% | 0.00% | 8.00% | 4.82% | 78.69% |
   | rl022| 20.95% | 0.00% | 13.46% | 2.64% | 78.06% |
   | rl024| 20.14% | 0.00% | 3.57% | 2.48% | 81.30% |
   | rl026| 31.25% | 5.56% | 1.47% | 2.52% | 75.52% |
   | rl028| 17.62% | 3.23% | 1.67% | 1.89% | 85.42% |
   | rl030| 22.36% | 3.77% | 2.94% | 1.99% | 82.63% |
   | rl032| 22.45% | 7.04% | 12.12% | 2.21% | 75.53% |
   | rl034| 18.48% | 0.00% | 7.14% | 2.68% | 78.32% |
   | rl036| 22.75% | 1.00% | 2.02% | 1.66% | 83.39% |
   | rl038| 10.90% | 1.79% | 7.27% | 1.82% | 85.09% |
   | rl040| 19.08% | 1.74% | 0.88% | 2.47% | 86.01% |
   | rl042| 23.95% | 3.00% | 5.15% | 3.59% | 79.71% |
   | rl044| 25.95% | 2.94% | 5.30% | 2.70% | 81.95% |
   | rl046| 16.23% | 5.26% | 4.17% | 1.59% | 83.10% |
   | rl048| 20.99% | 8.57% | 7.29% | 2.26% | 80.38% |
   | rl050| 16.08% | 0.81% | 1.63% | 1.05% | 89.69% |
   | sb002| 2.31% | 8.57% | 0.00% | 2.59% | 92.53% |
   | sb004| 15.87% | % | % | % | % |
   | sb006| % | % | % | % | % |
   | sb008| % | % | % | % | % |
   | sb010| % | % | % | % | % |
   | sb012| % | % | % | % | % |
   | sb014| % | % | % | % | % |
   | sb016| % | % | % | % | % |
   | sb018| % | % | % | % | % |
   | sb020| % | % | % | % | % |
   | sb022| % | % | % | % | % |
   | sb024| 9.22% | 8.47% | 3.70% | 2.35% | 86.76% |
   | sb026| 9.68% | 13.58% | 0.00% | 2.25% | 85.36% |
   | sb028| 9.31% | 6.92% | 1.65% | 1.75% | 89.37% |
   | sb030| 8.44% | 6.19% | 9.43% | 2.63% | 88.03% |
   | sb032| 18.60% | 7.61% | 2.35% | 2.14% | 80.11% |
   | sb034| 17.37% | 6.49% | 0.00% | 1.89% | 82.42% |
   | sb036| 10.61% | 8.09% | 1.60% | 2.52% | 87.58% |
   | sb038| 12.44% | 10.81% | 4.55% | 2.45% | 82.90% |
   | sb040| 15.13% | 2.61% | 0.89% | 2.63% | 87.66% |
   | sb042| 13.16% | 12.17% | 0.99% | 2.13% | 85.03% |
   | sb044| 9.74% | 5.04% | 0.76% | 2.45% | 89.67% |
   | sb046| 14.13% | 10.84% | 4.05% | 2.28% | 82.70% |
   | sb048| 14.47% | 15.65% | 1.03% | 2.02% | 82.85% |
   | sb050| 14.18% | 6.02% | 2.40% | 1.34% | 88.12% |
   | **Summary**| 15.94% | 6.64% | 3.20% | 2.40% | 84.37% |

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
