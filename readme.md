# IA Explicável para modelos clássicos e convolucionais

#### Aluno: [Luiz Fernando da Costa Castro](https://github.com/lfcastro95)
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler).

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](https://github.com/lfcastro95/Explainable-AI-for-Classic-and-Convolutional-Models).

### Resumo

Este trabalho apresenta uma análise comparativa entre abordagens clássicas e baseadas em redes neurais convolucionais (CNN) para classificação de imagens reais, multiclasse, de diferentes espécies de plantas. Foi aplicada uma metodologia de Explainable AI (XAI) para interpretar os resultados de ambos os modelos, utilizando técnicas como Permutação e Grad-CAM.

Imagens reais foram utilizadas e passaram por processo de data augmentation com o objetivo de aumentar a robustez do modelo. O projeto mostra que, mesmo com estratégias adequadas de regularização (L1/L2), aumento de dados e fine-tuning, redes profundas ainda tem dificuldade para superar modelos clássicos em acurácia, especialmente quando esses possuem dados tabulares bem documentados e estruturados.

Apesar da acurácia menor do modelo convolucional, é impressionante notar que mesmo com poucas imagens disponíveis ele atingiu resultados satisfatórios. Por um lado, o modelo clássico depende de medições precisas e de trabalho mais intensivo para tabular os dados, por outro, o modelo de redes convolucionais precisa de mais trabalho no refinamento do modelo e de otimização de parâmetros para que alcance bons resultados.

### 1. Introdução

Os modelos de  classificação de espécies são um problema clássico e bastante documentado. Modelos clássicos de Machine Learning lidam com isso há muitas décadas e há muitos anos os Modelos Convolucionais entraram em cena para resolver problemas semelhantes a partir de uma perspectiva diferente.

O escopo desse trabalho não é avaliar os melhores modelos, parâmetros e técnicas. Portanto, para o modelo clássico utilizado, optou-se por um já bem reconhecido pela sua capacidade de lidar com esse tipo de dataset. Já para o modelo convolucional os métodos descritos anteriormente foram utilizados não para encontrar o melhor modelo ou os melhores parâmetros possíveis, mas para chegar-se a uma acurácia satisfatória que permitisse aprofundar a análise.

### 2. Datasets

Para uma análise comparativa efetiva foram priorizados datasets parecidos. Assim, optou-se pela clássica base de dados de Iris e por uma versão alternativa composta por imagens de Iris. Ambas possuem as mesmas classes para classificação:

* Iris-setosa
* Iris-versicolor
* Iris-virginica

### 3. Modelagem de Treinamento

A modelagem foi dividida em duas partes:

* Modelo Clássico: foi utilizado Machine Learning tradicional baseado no Random Forest. O modelo apresenta ampla usabilidade nos problemas de classificação supervisionado e se mostra especialmente útil para categorização de objetos com base em medições precisas. Além de ser bem documentado seu uso no dataset de Iris. A explicabilidade foi conduzida via Permutabilidade.

* Modelo Rede Neural Convolucional: foi implementada uma arquitetura baseada no modelo pré-treinado MobileNetV2. Para aumentar a base de imagens foram utilizadas diversas técnicas de augmentation. Os parâmetros definidos para treinamento foram de 64 neurônios para última camada, ativação por ReLU e regularização L1/L2. Além disso, para predições, o modelo utiliza ativação por Softmax. Grad-CAM foi utilizado para inspecionar visualmente as ativações relevantes nas imagens.

### 4. Modelagem da IA Explicável

* Permutação e Gini: O método de Random Forest possui limitações na explicação via SHAP, portanto foram priorizados os métodos de Permutação e Gini para explicar a importância dos parâmetros de classificação. O Gini mede a impureza de cada nó de decisão. 0 indica um nó puro, onde as amostras pertencem a uma classe, enquanto 0,5 ou mais indica nós com classes mais distribuídas. Já a Permutação substitui aleatoriamente alguns dados dos parâmetros para mostrar o quanto eles influenciam na classificação correta. Assim, se um parâmetro apresenta variação de 0.2, indica que esse parâmetro influencia em 20% para a classificação final do modelo.

* Grad-CAM: A Classificação por Redes Neurais Convolucionais possui o desafio de entender o que o modelo utiliza para classificar as imagens. Portanto, a Grad-CAM nos permite visualizar de forma intuitiva o que os neurônios "veem". No modelo supervisionado, quanto mais vermelha e escura a região, indica que, com base no dataset de treino, os neurônios prestaram mais atenção em determinada área ao tentar classificar corretamente.

### 3. Resultados

O modelo clássico superou o modelo CNN em acurácia. Apesar das técnicas de data augmentation utilizadas, do oversampling, dos métodos de regularização e do fine-tuning, o modelo CNN apresentou resultado final inferior ao modelo de Machine Learning tradicional.

Conforme o escopo deste trabalho, buscamos a explicação para essa diferença de acurácia na forma que os modelos treinam e na importância dos parâmetros na tomada de decisão. Para o modelo clássico, identificamos acurácia próxima a 100%, enquanto o modelo CNN teve acurácia de aproximadamente 80%.

Dois parâmetros do modelo clássico são determinantes para a tomada de decisão: o comprimento e a largura da pétala. Juntos eles correspondem a cerca de 80% do grau de importância Gini e possuem entre 10% a 20% de perda de acurácia quando seus valores são permutados aleatoriamente.

Quanto ao modelo CNN, não é possível utilizarmos técnica semelhante ao modelo clássico para identificação da importância dos parâmetros. Por ser baseado no treinamento de camadas, padrões espaciais e neurônios, até seria possível verificar quais filtros foram mais ativados e a importância de cada neurônio, mas os resultados seriam pouco interpretativos para seres humanos e portanto, ainda que seja possível, é inutilizável para explicação. O Grad-CAM nos permite ver de forma mais humana como cada imagem foi analisada.

A explicabilidade dos modelos é matematicamente mais clara no modelo clássico, uma vez que esse modelo se baseia em dados tabulares para tomar suas decisões e a permutação nos permite montar gráficos que esclarecem essa diferença. Visualmente, no entanto, o modelo Convolucional se aproxima da interpretação humana e seus resultados são bastante acurados. O Grad-CAM mostra que o modelo acertou por distinguir corretamente as plantas do entorno e por identificar as diferenças entre os diferentes tipos de Iris com base no seu treinamento. Já nos casos errados, o modelo identificou o entorno como área de atenção para a classificação ou identificou a região da Iris, mas não o suficiente para distinguir entre as três espécies.

### 4. Conclusões

Apesar do desempenho inferior do modelo de Rede Neural Convolucional, é notável observar a evolução da computação para identificação de imagens. Os dados tabulares fornecidos para o modelo clássico são mais eficientes para a classificação correta de espécies, contudo nem sempre temos disponível esse tipo de informação. Além de exigir trabalho minucioso de especialistas e significativo esforço humano para medições precisas que não impactem na qualidade da informação. O modelo clássico, ainda que mais preciso, é suscetível ao erro e eficiência humana.

O modelo Convolucional se destaca pela possibilidade de pegar imagens reais e classificá-las de forma automática e escalável. Uma vez devidamente treinada e com técnica adequada de generalização, essa rede é capaz de reconhecer espécies com elevada precisão. Além disso, a possibilidade de pegarmos arquiteturas pré-treinadas robustas, como o MobileNetV2, e transferir seu conhecimento (transfer learning) para uma base de dados inteiramente nova, tendo apenas que realizar fine-tuning no treinamento das últimas épocas, representa uma economia de tempo e recursos importante.

Do ponto de vista da explicabilidade dos modelos, o modelo clássico é inegavelmente mais claro sobre as fronteiras de cada classe e os impactos de cada parâmetro na classificação final. Entretanto, o Convolucional tem uma explicação mais "humana" de classificar. Ao nos mostrar na imagem as áreas de maior atenção dos neurônios para chegar ao resultado final, ele se aproxima do racionínio visual humano e nos possibilita ver claramente se ele observou a área correta e o que é visualmente importante para tomar uma decisão.

---

Matrícula: 231.101.037

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
