<div align="center">
<img src="https://tensorflow.org/images/SIGAddons.png" width="60%"><br><br>
</div>

---

# TensorFlow  retretr<br>Addons

**TensorFlow Addons** is a repository of contributions that conform to well-established <br>API patterns, but implement new functionality not available in core TensorFlow. TensorFlow natively supports a large number of operators, layers, metrics, losses, and optimizers. However, in a fast moving field like ML, there are many interesting new developments that cannot be integrated into core TensorFlow (because their broad applicability is not yet clear, or it is mostly used by a smaller subset of the community).

## Installation

#### Stabile Konstruktionen

Um die neueste Version zu installieren, führen Sie Folgendes aus:

```
pip install tensorflow-addons
j
```

So verwenden Sie Add-ons:

```python
import tensorflow as tf
import tensorflow_addons as tfa
```

#### Nächtliche Builds

There are also nightly builds of TensorFlow Addons under the pip package `tfa-nightly`, which is built against the latest stable version of TensorFlow. <br>Nightly builds include newer features, but may be less stable than the versioned releases.

```
pip install tfa-nightly
```

#### Von der Quelle installieren

Sie können auch von der Quelle installieren. Dies erfordert das [Bazel-](https://bazel.build/) Build-System.

```
git clone https://github.com/tensorflow/addons.git
cd addons

# If building GPU Ops (Requires CUDA 10.0 and CuDNN 7)

export TF_NEED_CUDA=1
export CUDA_TOOLKIT_PATH="/path/to/cuda10" (default: /usr/local/cuda)
export CUDNN_INSTALL_PATH="/path/to/cudnn" (default: /usr/lib/x86_64-linux-gnu)

# This script links project with TensorFlow dependency

python3 ./configure.py

bazel build build_pip_pkg
bazel-bin/build_pip_pkg artifacts

pip install artifacts/tensorflow_addons-*.whl
```

## Kernkonzepte

#### Standardisierte API innerhalb von Unterpaketen

Benutzererfahrung und Wartbarkeit des Projekts sind Kernkonzepte in TF-Addons. Um dies zu erreichen, müssen unsere Ergänzungen den etablierten API-Mustern entsprechen, die im TensorFlow-Kern zu sehen sind.

#### Benutzerdefinierte GPU/CPU-Operationen

Ein großer Vorteil von TensorFlow-Add-ons besteht darin, dass es vorkompilierte Operationen gibt. Sollte keine CUDA 10-Installation gefunden werden, greift der Vorgang automatisch auf eine CPU-Implementierung zurück.

#### Proxy-Wartung

Addons wurden entwickelt, um Unterpakete und Untermodule zu unterteilen, sodass sie von Benutzern verwaltet werden können, die über Fachwissen und ein begründetes Interesse an dieser Komponente verfügen.

Die Betreuerschaft für Unterpakete wird nur gewährt, nachdem ein wesentlicher Beitrag geleistet wurde, um die Anzahl der Benutzer mit Schreibberechtigung zu begrenzen. Beiträge können in Form von Problembehebungen, Fehlerbehebungen, Dokumentation, neuem Code oder der Optimierung vorhandenen Codes erfolgen. Die Submodul-Betreuerschaft kann mit einer niedrigeren Eintrittsbarriere gewährt werden, da dies keine Schreibberechtigungen für das Repo beinhaltet.

Weitere Informationen finden Sie [im RFC](https://github.com/tensorflow/community/blob/master/rfcs/20190308-addons-proxy-maintainership.md) zu diesem Thema.

#### Regelmäßige Bewertung von Unterpaketen

Aufgrund der Beschaffenheit dieses Repositorys können Unterpakete und Untermodule im Laufe der Zeit für die Community immer weniger nützlich werden. Um die Nachhaltigkeit des Repositorys zu gewährleisten, führen wir alle zwei Jahre Überprüfungen unseres Codes durch, um sicherzustellen, dass alles noch in das Repository gehört. Zu dieser Überprüfung beitragende Faktoren sind:

1. Anzahl der aktiven Betreuer
2. Umfang der OSS-Nutzung
3. Anzahl der Probleme oder Fehler, die dem Code zugeschrieben werden
4. Wenn es jetzt eine bessere Lösung gibt

Die Funktionalität innerhalb von TensorFlow Addons kann in drei Gruppen eingeteilt werden:

- **Empfohlen** : gut gepflegte API; Die Nutzung ist erwünscht.
- **Entmutigt** : Es gibt eine bessere Alternative. die API wird aus historischen Gründen beibehalten; oder die API erfordert Wartung und ist die Wartezeit, bis sie veraltet ist.
- **Veraltet** : Nutzung auf eigene Gefahr; Betreff, der gelöscht werden soll.

Die Statusänderung zwischen diesen drei Gruppen ist: Vorgeschlagen &lt;-&gt; Entmutigt -&gt; Veraltet.

Der Zeitraum zwischen der Markierung einer API als veraltet und der Löschung beträgt 90 Tage. Die Begründung lautet:

1. Für den Fall, dass TensorFlow Addons monatlich veröffentlicht werden, wird es zwei bis drei Veröffentlichungen geben, bevor eine API gelöscht wird. Die Versionshinweise könnten dem Benutzer ausreichend Warnungen geben.

2. 90 Tage geben den Betreuern ausreichend Zeit, ihren Code zu reparieren.

## Mitwirken

TF-Addons ist ein von der Community geführtes Open-Source-Projekt. Daher ist das Projekt auf öffentliche Beiträge, Fehlerbehebungen und Dokumentation angewiesen. Eine [Anleitung zum Spenden finden Sie in den Beitragsrichtlinien](https://github.com/tensorflow/addons/blob/master/CONTRIBUTING.md) . Dieses Projekt hält sich an [den Verhaltenskodex von TensorFlow](https://github.com/tensorflow/addons/blob/master/CODE_OF_CONDUCT.md) . Durch Ihre Teilnahme wird von Ihnen erwartet, dass Sie diesen Kodex einhalten.

## Gemeinschaft

- [Öffentliche Mailingliste](https://groups.google.com/a/tensorflow.org/forum/#!forum/addons)
- [SIG-Monatsbesprechungsnotizen](https://docs.google.com/document/d/1kxg5xIHWLY7EMdOJCdSGgaPu27a9YKpupUz2VTXqTJg)
    - Treten Sie unserer Mailingliste bei und erhalten Sie Kalendereinladungen zum Meeting

## Lizenz

[Apache-Lizenz 2.0](LICENSE)
