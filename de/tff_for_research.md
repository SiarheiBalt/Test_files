# Verwendung von TFF für föderierte Lernforschung

<!-- Note that some section headings are used as deep links into the document.
     If you update those section headings, please make sure you also update
     any links to the section. -->

## Überblick

TFF ist ein erweiterbares, leistungsstarkes Framework für die Durchführung von Federated-Learning-Forschung (FL), indem föderierte Berechnungen anhand realistischer Proxy-Datensätze simuliert werden. Auf dieser Seite werden die wichtigsten Konzepte und Komponenten beschrieben, die für Forschungssimulationen relevant sind, sowie detaillierte Anleitungen für die Durchführung verschiedener Arten von Forschung in TFF.

## Die typische Struktur des Forschungscodes in TFF

Eine in TFF implementierte Forschungs-FL-Simulation besteht typischerweise aus drei Haupttypen von Logik.

1. Einzelne Teile des TensorFlow-Codes, typischerweise `tf.function` s, die Logik kapseln, die an einem einzelnen Ort ausgeführt wird (z. B. auf Clients oder auf einem Server). Dieser Code wird normalerweise ohne `tff.*` Referenzen geschrieben und getestet und kann außerhalb von TFF wiederverwendet werden. Auf dieser Ebene wird beispielsweise die [Client-Trainingsschleife in Federated Averaging](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tf.py#L184-L222) implementiert.

2. TensorFlow Federated-Orchestrierungslogik, die die einzelnen `tf.function` s aus 1. zusammenbindet, indem sie sie als `tff.tf_computation` s umschließt und sie dann mithilfe von Abstraktionen wie `tff.federated_broadcast` und `tff.federated_mean` innerhalb einer `tff.federated_computation` orchestriert. Sehen Sie sich zum Beispiel diese [Orchestrierung für Federated Averaging](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tff.py#L112-L140) an.

3. Ein äußeres Treiberskript, das die Steuerlogik eines Produktions-FL-Systems simuliert, simulierte Clients aus einem Datensatz auswählt und dann die in 2. definierten Verbundberechnungen auf diesen Clients ausführt. Zum Beispiel [ein Federated EMNIST-Experimenttreiber](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/emnist_fedavg_main.py) .

## Föderierte Lerndatensätze

TensorFlow föderiert [hostet mehrere Datensätze](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets) , die die Merkmale realer Probleme darstellen, die mit föderiertem Lernen gelöst werden könnten.

Hinweis: Diese Datensätze können auch von jedem Python-basierten ML-Framework als Numpy-Arrays genutzt werden, wie in der [ClientData-API](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/ClientData) dokumentiert.

Zu den Datensätzen gehören:

- [**StackOverflow** .](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/stackoverflow/load_data) Ein realistischer Textdatensatz für Sprachmodellierung oder überwachte Lernaufgaben mit 342.477 einzelnen Benutzern und 135.818.730 Beispielen (Sätzen) im Trainingssatz.

- [**Föderierter EMNIST** .](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/emnist/load_data) Eine föderierte Vorverarbeitung des EMNIST-Zeichen- und Zifferndatensatzes, bei der jeder Client einem anderen Autor entspricht. Das vollständige Zugset enthält 3400 Benutzer mit 671.585 Beispielen von 62 Marken.

- [**Shakespeare** .](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/shakespeare/load_data) Ein kleinerer Textdatensatz auf Zeichenebene, der auf den Gesamtwerken von William Shakespeare basiert. Der Datensatz besteht aus 715 Benutzern (Figuren aus Shakespeare-Stücken), wobei jedes Beispiel einem zusammenhängenden Satz von Zeilen entspricht, die von der Figur in einem bestimmten Stück gesprochen werden.

- [**CIFAR-100** .](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/cifar100/load_data) Eine föderierte Partitionierung des CIFAR-100-Datensatzes auf 500 Trainings-Clients und 100 Test-Clients. Jeder Kunde hat 100 einzigartige Beispiele. Die Partitionierung erfolgt so, dass eine realistischere Heterogenität zwischen den Clients entsteht. Weitere Einzelheiten finden Sie in der [API](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/cifar100/load_data) .

- [**Google Landmark v2-Datensatz**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/gldv2/load_data) Der Datensatz besteht aus Fotos verschiedener Wahrzeichen der Welt, wobei die Bilder nach Fotografen gruppiert sind, um eine gemeinsame Partitionierung der Daten zu erreichen. Es stehen zwei Arten von Datensätzen zur Verfügung: ein kleinerer Datensatz mit 233 Kunden und 23.080 Bildern und ein größerer Datensatz mit 1.262 Kunden und 164.172 Bildern.

- [**CelebA**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/celeba/load_data) Ein Datensatz mit Beispielen (Bild- und Gesichtsattribute) von Prominentengesichtern. Im Verbunddatensatz sind die Beispiele jeder Berühmtheit gruppiert, um einen Kunden zu bilden. Es gibt 9343 Clients mit jeweils mindestens 5 Beispielen. Der Datensatz kann entweder nach Kunden oder nach Beispielen in Trainings- und Testgruppen aufgeteilt werden.

- [**iNaturalist**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/inaturalist/load_data) Ein Datensatz besteht aus Fotos verschiedener Arten. Der Datensatz enthält 120.300 Bilder für 1.203 Arten. Es stehen sieben Varianten des Datensatzes zur Verfügung. Einer davon wird nach dem Fotografen gruppiert und besteht aus 9257 Kunden. Die restlichen Datensätze sind nach dem geografischen Standort gruppiert, an dem das Foto aufgenommen wurde. Diese sechs Varianten des Datensatzes bestehen aus 11 bis 3.606 Kunden.

## Hochleistungssimulationen

Während die Arbeitszeit einer FL- *Simulation* keine relevante Messgröße für die Bewertung von Algorithmen ist (da Simulationshardware nicht repräsentativ für echte FL-Bereitstellungsumgebungen ist), ist die schnelle Ausführung von FL-Simulationen für die Forschungsproduktivität von entscheidender Bedeutung. Daher hat TFF stark in die Bereitstellung leistungsstarker Laufzeiten für einzelne und mehrere Maschinen investiert. Die Dokumentation befindet sich in der Entwicklung, aber sehen Sie sich zunächst die Anweisungen zu [TFF-Simulationen mit Beschleunigern](https://www.tensorflow.org/federated/tutorials/simulations_with_accelerators) und die Anweisungen zum [Einrichten von Simulationen mit TFF auf GCP](https://www.tensorflow.org/federated/gcp_setup) an. Die Hochleistungs-TFF-Laufzeit ist standardmäßig aktiviert.

## TFF für verschiedene Forschungsbereiche

### Föderierte Optimierungsalgorithmen

Die Forschung zu föderierten Optimierungsalgorithmen kann in TFF je nach gewünschtem Grad der Anpassung auf unterschiedliche Weise durchgeführt werden.

Eine minimale eigenständige Implementierung des [Federated Averaging-](https://arxiv.org/abs/1602.05629) Algorithmus wird [hier](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg) bereitgestellt. Der Code enthält [TF-Funktionen](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tf.py) für lokale Berechnungen, [TFF-Berechnungen](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tff.py) für die Orchestrierung und ein [Treiberskript](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/emnist_fedavg_main.py) für den EMNIST-Datensatz als Beispiel. Diese Dateien können leicht an benutzerdefinierte Anwendungen und algorithmische Änderungen angepasst werden, indem detaillierte Anweisungen in der [README-Datei](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/README.md) befolgt werden.

Eine allgemeinere Implementierung von Federated Averaging finden Sie [hier](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/python/learning/algorithms/fed_avg.py) . Diese Implementierung ermöglicht ausgefeiltere Optimierungstechniken, einschließlich der Verwendung verschiedener Optimierer sowohl auf dem Server als auch auf dem Client. Weitere föderierte Lernalgorithmen, einschließlich föderiertem K-Means-Clustering, finden Sie [hier](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/python/learning/algorithms/) .

### Komprimierung der Modellaktualisierung

Die verlustbehaftete Komprimierung von Modellaktualisierungen kann zu geringeren Kommunikationskosten führen, was wiederum zu einer kürzeren Gesamttrainingszeit führen kann.

Um eine aktuelle [Arbeit](https://arxiv.org/abs/2201.02664) zu reproduzieren, sehen Sie sich [dieses Forschungsprojekt](https://github.com/google-research/federated/tree/master/compressed_communication) an. Informationen zum Implementieren eines benutzerdefinierten Komprimierungsalgorithmus finden Sie unter [„comparency_methods“](https://github.com/google-research/federated/tree/master/compressed_communication/aggregators/comparison_methods) im Projekt für Baselines als Beispiel und im [TFF-Aggregators-Tutorial](https://www.tensorflow.org/federated/tutorials/custom_aggregators) , falls Sie noch nicht damit vertraut sind.

### Differenzierte Privatsphäre

TFF ist mit der [TensorFlow Privacy-](https://github.com/tensorflow/privacy) Bibliothek interoperabel, um die Erforschung neuer Algorithmen für das föderierte Training von Modellen mit differenzieller Privatsphäre zu ermöglichen. Ein Beispiel für das Training mit DP unter Verwendung [des grundlegenden DP-FedAvg-Algorithmus](https://arxiv.org/abs/1710.06963) und [Erweiterungen](https://arxiv.org/abs/1812.06210) finden Sie [in diesem Experimenttreiber](https://github.com/google-research/federated/blob/master/differential_privacy/stackoverflow/run_federated.py) .

Wenn Sie einen benutzerdefinierten DP-Algorithmus implementieren und auf die Aggregataktualisierungen der föderierten Mittelwertbildung anwenden möchten, können Sie einen neuen DP-Mittelwertalgorithmus als Unterklasse von [`tensorflow_privacy.DPQuery`](https://github.com/tensorflow/privacy/blob/master/tensorflow_privacy/privacy/dp_query/dp_query.py#L54) implementieren und eine `tff.aggregators.DifferentiallyPrivateFactory` mit einer Instanz Ihrer Abfrage erstellen. Ein Beispiel für die Implementierung des [DP-FTRL-Algorithmus](https://arxiv.org/abs/2103.00039) finden Sie [hier](https://github.com/google-research/federated/blob/master/dp_ftrl/dp_fedavg.py)

Föderierte GANs ( [unten](#generative_adversarial_networks) beschrieben) sind ein weiteres Beispiel für ein TFF-Projekt, das differenziellen Datenschutz auf Benutzerebene implementiert (z. B. [hier im Code](https://github.com/google-research/federated/blob/master/gans/tff_gans.py#L144) ).

### Robustheit und Angriffe

TFF kann auch verwendet werden, um die gezielten Angriffe auf föderierte Lernsysteme und unterschiedliche datenschutzbasierte Abwehrmaßnahmen zu simulieren, die in *[„Können Sie Federated Learning wirklich hinter die Tür setzen?“](https://arxiv.org/abs/1911.07963)* betrachtet werden. . Dies geschieht durch den Aufbau eines iterativen Prozesses mit potenziell bösartigen Clients (siehe [`build_federated_averaging_process_attacked`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/attacked_fedavg.py#L412) ). Das Verzeichnis [„targeted_attack“](https://github.com/tensorflow/federated/tree/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack) enthält weitere Details.

- Neue Angriffsalgorithmen können implementiert werden, indem eine Client-Update-Funktion geschrieben wird, die eine Tensorflow-Funktion ist. Ein Beispiel finden Sie unter [`ClientProjectBoost`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/attacked_fedavg.py#L460) .
- Neue Abwehrmaßnahmen können durch Anpassen von [„tff.utils.StatefulAggregateFn“](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/tensorflow_federated/python/core/utils/computation_utils.py#L103) implementiert werden, das Client-Ausgaben aggregiert, um ein globales Update zu erhalten.

Ein Beispielskript für die Simulation finden Sie unter [`emnist_with_targeted_attack.py`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/emnist_with_targeted_attack.py) .

### Generative gegnerische Netzwerke

GANs sorgen für ein interessantes [föderiertes Orchestrierungsmuster](https://github.com/google-research/federated/blob/master/gans/tff_gans.py#L266-L316) , das etwas anders aussieht als die standardmäßige Federated Averaging. Sie umfassen zwei unterschiedliche Netzwerke (den Generator und den Diskriminator), die jeweils mit ihrem eigenen Optimierungsschritt trainiert werden.

TFF kann für die Forschung zum föderierten Training von GANs verwendet werden. Beispielsweise ist der in [der jüngsten Arbeit](https://arxiv.org/abs/1911.06679) vorgestellte DP-FedAvg-GAN-Algorithmus [in TFF implementiert](https://github.com/tensorflow/federated/tree/main/federated_research/gans) . Diese Arbeit zeigt die Wirksamkeit der Kombination von föderiertem Lernen, generativen Modellen und [differenzieller Privatsphäre](#differential_privacy) .

### Personalisierung

Personalisierung im Rahmen des föderierten Lernens ist ein aktives Forschungsgebiet. Das Ziel der Personalisierung besteht darin, verschiedenen Benutzern unterschiedliche Inferenzmodelle bereitzustellen. Es gibt möglicherweise unterschiedliche Ansätze für dieses Problem.

Ein Ansatz besteht darin, jedem Kunden die Feinabstimmung eines einzelnen globalen Modells (trainiert durch föderiertes Lernen) mit seinen lokalen Daten zu überlassen. Dieser Ansatz weist Verbindungen zum Meta-Lernen auf, siehe z. B. [diesen Artikel](https://arxiv.org/abs/1909.12488) . Ein Beispiel für diesen Ansatz finden Sie in [`emnist_p13n_main.py`](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/personalization/emnist_p13n_main.py) . Um verschiedene Personalisierungsstrategien zu erkunden und zu vergleichen, können Sie:

- Definieren Sie eine Personalisierungsstrategie, indem Sie eine `tf.function` implementieren, die von einem anfänglichen Modell ausgeht, ein personalisiertes Modell trainiert und bewertet, indem es die lokalen Datensätze jedes Kunden verwendet. Ein Beispiel liefert [`build_personalize_fn`](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/personalization/p13n_utils.py) .

- Definieren Sie ein `OrderedDict` , das Strategienamen den entsprechenden Personalisierungsstrategien zuordnet, und verwenden Sie es als Argument `personalize_fn_dict` in [`tff.learning.build_personalization_eval`](https://www.tensorflow.org/federated/api_docs/python/tff/learning/build_personalization_eval) .

Ein anderer Ansatz besteht darin, das Training eines vollständig globalen Modells zu vermeiden, indem ein Teil eines Modells vollständig lokal trainiert wird. Eine Instanziierung dieses Ansatzes wird in [diesem Blogbeitrag](https://ai.googleblog.com/2021/12/a-scalable-approach-for-partially-local.html) beschrieben. Dieser Ansatz ist auch mit Meta-Lernen verbunden, siehe [dieses Papier](https://arxiv.org/abs/2102.03448) . Um teilweise lokales föderiertes Lernen zu erkunden, können Sie:

- Schauen Sie sich das [Tutorial](https://www.tensorflow.org/federated/tutorials/federated_reconstruction_for_matrix_factorization) an, um ein vollständiges Codebeispiel für die Anwendung von Federated Reconstruction und [Folgeübungen](https://www.tensorflow.org/federated/tutorials/federated_reconstruction_for_matrix_factorization#further_explorations) zu erhalten.

- Erstellen Sie einen teilweise lokalen Trainingsprozess mit [`tff.learning.reconstruction.build_training_process`](https://www.tensorflow.org/federated/api_docs/python/tff/learning/reconstruction/build_training_process) und ändern Sie `dataset_split_fn` , um das Prozessverhalten anzupassen.
