Table of content

[StackMix and Blot Augmentations for Handwritten Text Recognition](#stackmix)  
[Connectionist Temporal Classification (CTC)](#ctc)  
[Attention-basedFullyGatedCNN-BGRU](#Attention_basedFullyGatedCNN_BGRU)  

<a name="stackmix"></a> 
# StackMix and Blot Augmentations for Handwritten Text Recognition
paper [arxiv](https://arxiv.org/abs/2108.11667)  [paperswithcode](https://paperswithcode.com/paper/stackmix-and-blot-augmentations-for)

Datasets Used [IAM](https://paperswithcode.com/dataset/iam) [HKR](https://paperswithcode.com/dataset/hkr) [HKR-Dataset](https://github.com/abdoelsayed2016/HKR_Dataset) [Digital-Peter](https://paperswithcode.com/dataset/digital-peter) [cyrillic-handwriting-dataset-kaggle](https://www.kaggle.com/datasets/constantinwerner/cyrillic-handwriting-dataset) [HKR-huggingface](https://huggingface.co/datasets/nastyboget/stackmix_hkr)

examples [StackMix-OCR-github](https://github.com/ai-forever/StackMix-OCR)

В тексте рассматриваются различные методы и подходы к распознаванию рукописного текста (HTR) с использованием моделей глубокого обучения. В начале излагаются первые работы по проблемам распознавания рукописного текста, в которых использовалась комбинация скрытых марковских моделей и рекуррентных нейронных сетей (RNN) или алгоритмы на основе условных случайных полей. Однако было обнаружено, что эти подходы имеют свои недостатки, в частности, невозможность оптимизации сквозной функции потерь.

В 2006 году был представлен новый подход, известный как коннекционистская темпоральная классификация (CTC). Этот подход интерпретирует выходные данные сети как распределение вероятностей по всем возможным последовательностям меток, обусловленным заданной входной последовательностью. Это позволяет получить целевую функцию, которая непосредственно максимизирует вероятности правильных меток. Поскольку объективная функция дифференцируема, сеть может быть обучена стандартным методом обратного распространения во времени. В рамках этого подхода были введены потери CTC, которые нашли широкое признание среди исследователей и стали стандартом де-факто для распознавания рукописных работ.

Также были упомянуты MDLSTM-сети, использующие 2D-RNN. Эти сети могут работать с обеими осями входного изображения и состоят из нескольких слоев CNN и MDLSTM. Однако эти модели имеют ряд недостатков, таких как высокая вычислительная стоимость и нестабильность. Для решения этих проблем были предложены различные методы, такие как метод "упаковки примеров" из работы [10] и исключение рекуррентных слоев в CNN-LSTM-CTC для уменьшения количества параметров ieeexplore.ieee.org.

В качестве альтернативы подходу RCNN-CTC были также предложены модели Seq2seq. Эти модели используют кодер для извлечения признаков из входного сигнала и декодер с механизмом внимания для последовательной выдачи выходного сигнала. Обычные приемы могут существенно улучшить качество HTR-моделей arxiv.org.

Далее в тексте рассматривается новый метод дополнения данных, имитирующий зачеркнутый текст, известный как Handwritten Blots. Этот метод был разработан в ходе анализа набора данных Digital Peter и предполагает использование аугментации Cutout [12] для создания эффекта, похожего на перечеркнутые символы. Реализация этого алгоритма предполагает использование алгоритма построения кривой Безье для сглаживания перехода кривой между точками arxiv.org.

Наконец, в тексте описывается предложенная модель, состоящая из трех частей: модифицированной архитектуры нейронной сети Resnet, нового метода аугментации, имитирующего зачеркнутый текст, и метода значительного увеличения объема обучающих данных за счет генерации нового текста в стиле текущего набора данных. Результаты, полученные при использовании этой архитектуры без дополнительных модификаций, приведены в табл. 4, а влияние дополнения Blot на метрики качества также обсуждается arxiv.org.

<a name="ctc"></a> 
# Connectionist Temporal Classification (CTC)
Начиная с 2007 года LSTM приобрела популярность и смогла вывести на новый уровень распознавание речи, показав существенное улучшение по сравнению с традиционными моделями.[6] В 2009 году появился подход классификации по рейтингу (англ. Connectionist temporal classification, CTC). Этот метод позволил рекуррентным сетям подключить анализ контекста при распознавании рукописного текста.[7] В 2014 году китайская энциклопедия и поисковая система Baidu, используя рекуррентные сети с обучением по CTC смогли поднять на новый уровень показатели Switchboard Hub5’00, опередив традиционные методы.

Connectionist Temporal Classification (CTC) - это метод, используемый для обучения рекуррентных нейронных сетей (RNN) для обработки последовательностей данных. Этот метод особенно полезен для задач, где данные представляют собой последовательности, такие как распознавание речи или рукописного текста [ru.wikipedia.org](https://ru.wikipedia.org/wiki/%D0%A0%D0%B5%D0%BA%D1%83%D1%80%D1%80%D0%B5%D0%BD%D1%82%D0%BD%D0%B0%D1%8F_%D0%BD%D0%B5%D0%B9%D1%80%D0%BE%D0%BD%D0%BD%D0%B0%D1%8F_%D1%81%D0%B5%D1%82%D1%8C).

CTC представляет новую функцию потерь, которая позволяет сетям RNN напрямую использовать обучение памяти несегментированных последовательностей. Это означает, что CTC может работать с последовательностями данных, которые не были предварительно разделены на отдельные сегменты или "блоки". Вместо этого, CTC позволяет сетям RNN учиться на основе всего входного последовательного потока данных [russianblogs.com](https://www.russianblogs.com/article/3121270981/).

В контексте CTC, "соединитель" относится к способности сети учиться на основе последовательности входных данных, а "временной" относится к способности сети учиться на основе последовательности во времени. "Классификация" означает, что CTC используется для задач классификации, где выходной результат - это определенный класс или категория.

Важно отметить, что CTC не предназначен для прогнозирования последовательности, поскольку он может только прогнозировать классификацию некоторых независимых меток. Однако, CTC обеспечивает мощный общий механизм для построения временных рядов и очень устойчив к временным и пространственным шумам.

Важным аспектом CTC является использование "пустой" метки. Это специальная метка, которая может быть выведена на RNN. Выход RNN - это вероятность всех меток. Это позволяет CTC учиться на основе всего входного последовательного потока данных, а не только на отдельных сегментах.

В целом, CTC представляет собой мощный инструмент для обучения рекуррентных нейронных сетей на последовательных данных. Он обеспечивает гибкость и устойчивость к шумам, что делает его полезным для многих задач обработки последовательностей.

Connectionist Temporal Classification (CTC) - это функция потерь, используемая для обучения рекуррентных нейронных сетей (RNN) для маркировки неразделенных входных последовательностей данных в обучении с учителем.

В контексте распознавания речи, например, с использованием типичной функции потерь кросс-энтропии, входной сигнал должен быть разделен на слова или подслова. Однако, используя функцию потерь CTC, достаточно предоставить одну последовательность меток для входной последовательности, и сеть учится как выравнивать, так и маркировать.

CTC позволяет добиться как упорядочивания, так и распознавания. Многие рекуррентные сети используют стеки данных, присущие CTC, чтобы найти матрицу весов, в которой вероятность последовательности меток в наборе образцов при соответствующем входном потоке сводится к максимуму.

В MXNet реализована функция CTC-loss, которая включена в стандартный пакет. Также есть возможность использовать функцию CTC-loss с помощью библиотеки Baidu's warp-ctc, что требует сборки обеих библиотек из исходного кода [mxnet.apache.org](https://mxnet.apache.org/versions/1.2.1/tutorials/speech_recognition/ctc.html).

В качестве примера, MXNet предоставляет пример использования CTC-loss с сетью LSTM для выполнения предсказания распознавания текста на изображениях CAPTCHA. Этот пример демонстрирует использование обоих вариантов CTC-loss, а также инференции после обучения с использованием контрольных точек символа и параметров сети.

<a name="Attention_basedFullyGatedCNN_BGRU"></a>
# Attention-basedFullyGatedCNN-BGRU
Наша модель ориентирована на извлечение кириллической символики в рукописном виде. Предлагаемая модель основана на архитектуре Attention-Gated-CNN-BGRU с небольшим количеством параметров (около 885,337), что обеспечивает высокую скорость распознавания, компактность и быстродействие, а также меньшую погрешность по сравнению с другими моделями. Алгоритм состоит из шести этапов, которые будут описаны следующим образом: 

  1. предварительная обработка: изменение размера с разбивкой на страницы (1024x128), компенсация освещенности и рекурсивная обработка изображений в файлах формата HDF5 

  2. извлечение характеристик с помощью CNN-слоев 

  3. механизм внимания Bahdanau, который заставляет модель обращать внимание на входные данные и соотносить их с выходными данными. 

  4. Сопоставление последовательности признаков с помощью RRN 

  5. Расчет потерь/декодирование в текст или формат (CTC) 6.Постобработка для улучшения конечного текста

## Модель
В данном разделе мы опишем модель, в которой происходит передача сигнала через GatedCNN, затем процесс внимания по Bahdanau, bi-directionalGRU и, наконец, матрицу выхода из GRU к коннекционистско-темпоральной классификации (CTC) для вычисления величины потери и декодирования матрицы вывода в конечном контексте.  
Модель состоит из четырех основных частей: энкодер, внимание, декодер и CTC. 

Энкодер 

Conventional blocks. Энкодер получает входной сигнал и генерирует векторы признаков. Эти векторы признаков содержат информацию и характеристики, которые представляют входной сигнал.

Энкодерная сеть состоит из 5 конволюционных блоков, которые соответствуют обучению извлечению релевантных признаков из изображений. В каждом блоке выполняется операция преобразования, в ходе которой в первом, втором, четвертом и шестом блоках применяется ядро свертки размера(3,3), а в третьем и пятом блоках -(2,4), затем применяются ParametricReLU и BatchNormalization, а для уменьшения overfitting применяется Dropout в некоторых слоях преобразования (с вероятностью dropout равной 0,2).

## Слой Gated Conventional Layer.

Идея gate controls состоит в том, чтобы передавать векторный признак на следующий слой. Слой Gatelayer рассматривает значение векторного признака в данной позиции, а также соседние головные значения и определяет, следует ли его удерживать или отбрасывать в данной позиции. Слой gate(g) реализуется как слой разрешения со слоем активации. Он добавляется к входной функции maps(x). Выходной сигнал газового механизма представляет собой поэлементное умножение (point-wise multiplication) входов и выходов затвора.

## Механизм внимания

Внимание - это механизм, который обеспечивает явное кодирование исходных последовательностей (h1, ...., hs), способствующих построению контекстного вектора (ct), а затем его использование декодером. Внимание позволяет модели понять, на какие закодированные изображения в источнике следует обратить внимание, и в какой степени при этом происходит предсказание целевой последовательности. Скрытое состояние последовательности источников получается из кодера на каждом шаге ввода, а не из скрытого состояния на последнем шаге.

В целевой последовательности для каждого выходного слова явно строится контекстный вектор (ct). Сначала с помощью нейросети каждое скрытое состояние кодера градуируется и нормируется на вероятность по отношению к скрытым состояниям кодировщиков. Наконец, вероятности используются для вычисления взвешенной суммы скрытых состояний кодера, чтобы предоставить контекстную информацию, которая должна использоваться в кодере. Слой внимания выдает результаты размером 128x 256.

## Decoder

Декодер представляет собой двунаправленную рекуррентную сеть (GRU), которая обрабатывает последовательность признаков и предсказывает последовательности символов. Вектор признаков содержит 256 признаков с шагом по времени, и текущая нейронная сеть распространяет информацию через эту последовательность. Реализация RNN вGRU используется в качестве механизма сглаживания в рекуррентных нейронных сетях (RNN), почти как расширенный модуль LSTM без выходного затвора. GRU пытается раскрыть материю убывающего градиента. AGRU'scans решает проблему сходящегося градиента с помощью шлюза обновления и шлюза сброса. Шлюз обновления управляет информацией, поступающей в память, а шлюз сброса - информацией, поступающей в память. Шлюзы обновления и сброса представляют собой два вектора, которые определяют, какая информация будет передана на выход. Они могут быть отменены, чтобы сохранить прошлые знания или удалить информацию, не имеющую отношения к прогнозированию.GRU похож на LSTM, но содержит меньшее количество параметров, поскольку не имеет достаточной скорости для вывода. Выходная последовательность RNN-слоя представляет собой матрицу размером 128x96.

## Connectionist Tempora Classification (CTC)

Нейронные сети имеют разные цели обучения для каждого участка входной последовательности на каждом временном шаге. Это имеет два важных следствия. Во-первых, это означает, что обучающие данные должны быть предварительно сегментированы для определения целей. Во-вторых, поскольку сеть генерирует только локальные классификации, глобальные аспекты последовательности (например, вероятность последовательного повторения меток) должны быть смоделированы извне. Действительно, конечная последовательность меток не может быть надежно определена без некоторой предварительной обработки. Это достигается за счет того, что при условии корректности общей последовательности меток в каждый момент времени на входе можно делать прогнозы по меткам. Это позволяет отказаться от предварительной сегментации базы данных, так как выравнивание меток по входу уже не имеет значения.

Кроме того, CTC напрямую предоставляет полные вероятности последовательности меток, что обеспечивает отсутствие необходимости дополнительной постобработки для использования сети в качестве временного классификатора. При обучении NN CTC получает выходную матрицу RNN и текст, содержащий ground truth, и вычисляет величину потерь. В процессе распознавания CTC получает матрицу и декодирует ее в конечный текст.

Функция потерь: Для заданного входного сигнала мы хотели бы обучить модель максимизации вероятности того, что ей будет присвоен правильный ответ. Для этого необходимо вычислить условную вероятностьp(Y|X). Функцияp(Y|X)должна также иметь дифференциалы, чтобы ее можно было использовать по возрастающей.
