# Using TFF for Federated Learning Research

<!-- Note that some section headings are used as deep links into the document.
     If you update those section headings, please make sure you also update
     any links to the section. -->

## Overview

TFF is an extensible, powerful framework for conducting federated learning (FL)
research by simulating federated computations on realistic proxy datasets. This
page describes the main concepts and components that are relevant for research
simulations, as well as detailed guidance for conducting different kinds of
research in TFF.

## The typical structure of research code in TFF

A research FL simulation implemented in TFF typically consists of three main
types of logic.

1.  Individual pieces of TensorFlow code, typically `tf.function`s, that
    encapsulate logic that runs in a single location (e.g., on clients or on a
    server). This code is typically written and tested without any `tff.*`
    references, and can be re-used outside of TFF. For example, the
    [client training loop in Federated Averaging](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tf.py#L184-L222)
    is implemented at this level.

1.  TensorFlow Federated orchestration logic, which binds together the
    individual `tf.function`s from 1. by wrapping them as `tff.tf_computation`s
    and then orchestrating them using abstractions like
    `tff.federated_broadcast` and `tff.federated_mean` inside a
    `tff.federated_computation`. See, for example, this
    [orchestration for Federated Averaging](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tff.py#L112-L140).

1.  An outer driver script that simulates the control logic of a production FL
    system, selecting simulated clients from a dataset and then executing
    federated computations defined in 2. on those clients. For example,
    [a Federated EMNIST experiment driver](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/emnist_fedavg_main.py).

## Federated learning datasets

TensorFlow federated
[hosts multiple datasets](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets)
that are representative of the characteristics of real-world problems that could
be solved with federated learning.

Note: These datasets can also be consumed by any Python-based ML framework as
Numpy arrays, as documented in the
[ClientData API](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/ClientData).

Datasets include:

- [**StackOverflow**.](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/stackoverflow/load_data)
  A realistic text dataset for language modeling or supervised learning tasks,
  with 342,477 unique users with 135,818,730 examples (sentences) in the
  training set.

- [**Federated EMNIST**.](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/emnist/load_data)
  A federated pre-processing of the EMNIST character and digit dataset, where
  each client corresponds to a different writer. The full train set contains
  3400 users with 671,585 examples from 62 labels.

- [**Shakespeare**.](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/shakespeare/load_data)
  A smaller char-level text dataset based on the complete works of William
  Shakespeare. The data set consists of 715 users (characters of Shakespeare
  plays), where each example corresponds to a contiguous set of lines spoken
  by the character in a given play.

- [**CIFAR-100**.](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/cifar100/load_data)
  A federated partitioning of the CIFAR-100 dataset across 500 training
  clients and 100 test clients. Each client has 100 unique examples. The
  partitioning is done in a way to create more realistic heterogeneity between
  clients. For more details, see the
  [API](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/cifar100/load_data).

- [**Google Landmark v2 dataset**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/gldv2/load_data)
  The dataset consists of photos of various world landmarks, with images
  grouped by photographer to achieve a federated partitioning of the data. Two
  flavors of dataset are available: a smaller dataset with 233 clients and
  23080 images, and a larger dataset with 1262 clients and 164172 images.

- [**CelebA**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/celeba/load_data)
  A dataset of examples (image and facial attributes) of celebrity faces. The
  federated dataset has each celebrity's examples grouped together to form a
  client. There are 9343 clients, each with at least 5 examples. The dataset
  can be split into train and test groups either by clients or by examples.

- [**iNaturalist**](https://www.tensorflow.org/federated/api_docs/python/tff/simulation/datasets/inaturalist/load_data)
  A dataset consists of photos of various species. The dataset contains
  120,300 images for 1,203 species. Seven flavors of the dataset are
  available. One of them is grouped by the photographer and it consists of
  9257 clients. The rest of the datasets are grouped by the geo location where
  the photo was taken. These six flavors of the dataset consists of 11 - 3,606
  clients.

## High performance simulations

While the wall-clock time of an FL _simulation_ is not a relevant metric for
evaluating algorithms (as simulation hardware isn't representative of real FL
deployment environments), being able to run FL simulations quickly is critical
for research productivity. Hence, TFF has invested heavily in providing
high-performance single and multi-machine runtimes. Documentation is under
development, but for now see the instructions on
[TFF simulations with accelerators](https://www.tensorflow.org/federated/tutorials/simulations_with_accelerators),
and instructions on
[setting up simulations with TFF on GCP](https://www.tensorflow.org/federated/gcp_setup).
The high-performance TFF runtime is enabled by default.

## TFF for different research areas

### Federated optimization algorithms

Research on federated optimization algorithms can be done in different ways in
TFF, depending on the desired level of customization.

A minimal stand-alone implementation of the
[Federated Averaging](https://arxiv.org/abs/1602.05629) algorithm is provided
[here](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg).
The code includes
[TF functions](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tf.py)
for local computation,
[TFF computations](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/simple_fedavg_tff.py)
for orchestration, and a
[driver script](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/emnist_fedavg_main.py)
on the EMNIST dataset as an example. These files can easily be adapted for
customized applciations and algorithmic changes following detailed instructions
in the
[README](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/simple_fedavg/README.md).

A more general implementation of Federated Averaging can be found
[here](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/python/learning/algorithms/fed_avg.py).
This implementation allows for more sophisticated optimization techniques,
including the use of different optimizers on both the server and client. Other
federated learning algorithms, including federated k-means clustering, can be
found
[here](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/python/learning/algorithms/).

### Model update compression

Lossy compression of model updates can lead to reduced communication costs,
which in turn can lead to reduced overall training time.

To reproduce a recent [paper](https://arxiv.org/abs/2201.02664), see
[this research project](https://github.com/google-research/federated/tree/master/compressed_communication).
To implement a custom compression algorithm, see
[comparison_methods](https://github.com/google-research/federated/tree/master/compressed_communication/aggregators/comparison_methods)
in the project for baselines as an example, and
[TFF Aggregators tutorial](https://www.tensorflow.org/federated/tutorials/custom_aggregators)
if not already familiar with.

### Differential privacy

TFF is interoperable with the
[TensorFlow Privacy](https://github.com/tensorflow/privacy) library to enable
research in new algorithms for federated training of models with differential
privacy. For an example of training with DP using
[the basic DP-FedAvg algorithm](https://arxiv.org/abs/1710.06963) and
[extensions](https://arxiv.org/abs/1812.06210), see
[this experiment driver](https://github.com/google-research/federated/blob/master/differential_privacy/stackoverflow/run_federated.py).

If you want to implement a custom DP algorithm and apply it to the aggregate
updates of federated averaging, you can implement a new DP mean algorithm as a
subclass of
[`tensorflow_privacy.DPQuery`](https://github.com/tensorflow/privacy/blob/master/tensorflow_privacy/privacy/dp_query/dp_query.py#L54)
and create a `tff.aggregators.DifferentiallyPrivateFactory` with an instance of
your query. An example of implementing the
[DP-FTRL algorithm](https://arxiv.org/abs/2103.00039) can be found
[here](https://github.com/google-research/federated/blob/master/dp_ftrl/dp_fedavg.py)

Federated GANs (described [below](#generative_adversarial_networks)) are another
example of a TFF project implementing user-level differential privacy (e.g.,
[here in code](https://github.com/google-research/federated/blob/master/gans/tff_gans.py#L144)).

### Robustness and attacks

TFF can also be used to simulate the targeted attacks on federated learning
systems and differential privacy based defenses considered in
_[Can You Really Back door Federated Learning?](https://arxiv.org/abs/1911.07963)_.
This is done by building an iterative process with potentially malicious clients
(see
[`build_federated_averaging_process_attacked`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/attacked_fedavg.py#L412)).
The
[targeted_attack](https://github.com/tensorflow/federated/tree/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack)
directory contains more details.

- New attacking algorithms can be implemented by writing a client update
  function which is a Tensorflow function, see
  [`ClientProjectBoost`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/attacked_fedavg.py#L460)
  for an example.
- New defenses can be implemented by customizing
  ['tff.utils.StatefulAggregateFn'](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/tensorflow_federated/python/core/utils/computation_utils.py#L103)
  which aggregates client outputs to get a global update.

For an example script for simulation, see
[`emnist_with_targeted_attack.py`](https://github.com/tensorflow/federated/blob/6477a3dba6e7d852191bfd733f651fad84b82eab/federated_research/targeted_attack/emnist_with_targeted_attack.py).

### Generative Adversarial Networks

GANs make for an interesting
[federated orchestration pattern](https://github.com/google-research/federated/blob/master/gans/tff_gans.py#L266-L316)
that looks a little different than standard Federated Averaging. They involve
two distinct networks (the generator and the discriminator) each trained with
their own optimization step.

TFF can be used for research on federated training of GANs. For example, the
DP-FedAvg-GAN algorithm presented in
[recent work](https://arxiv.org/abs/1911.06679) is
[implemented in TFF](https://github.com/tensorflow/federated/tree/main/federated_research/gans).
This work demonstrates the effectiveness of combining federated learning,
generative models, and [differential privacy](#differential_privacy).

### Personalization

Personalization in the setting of federated learning is an active research area.
The goal of personalization is to provide different inference models to
different users. There are potentially different approaches to this problem.

One approach is to let each client fine-tune a single global model (trained
using federated learning) with their local data. This approach has connections
to meta-learning, see, e.g., [this paper](https://arxiv.org/abs/1909.12488). An
example of this approach is given in
[`emnist_p13n_main.py`](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/personalization/emnist_p13n_main.py).
To explore and compare different personalization strategies, you can:

- Define a personalization strategy by implementing a `tf.function` that
  starts from an initial model, trains and evaluates a personalized model
  using each client's local datasets. An example is given by
  [`build_personalize_fn`](https://github.com/tensorflow/federated/blob/main/tensorflow_federated/examples/personalization/p13n_utils.py).

- Define an `OrderedDict` that maps strategy names to the corresponding
  personalization strategies, and use it as the `personalize_fn_dict` argument
  in
  [`tff.learning.build_personalization_eval`](https://www.tensorflow.org/federated/api_docs/python/tff/learning/build_personalization_eval).

Another approach is to avoid training a fully global model by training part of a
model entirely locally. An instantiation of this approach is described in
[this blog post](https://ai.googleblog.com/2021/12/a-scalable-approach-for-partially-local.html).
This approach is also connected to meta learning, see
[this paper](https://arxiv.org/abs/2102.03448). To explore partially local
federated learning, you can:

- Check out the
  [tutorial](https://www.tensorflow.org/federated/tutorials/federated_reconstruction_for_matrix_factorization)
  for a complete code example applying Federated Reconstruction and
  [follow-up exercises](https://www.tensorflow.org/federated/tutorials/federated_reconstruction_for_matrix_factorization#further_explorations).

- Create a partially local training process using
  [`tff.learning.reconstruction.build_training_process`](https://www.tensorflow.org/federated/api_docs/python/tff/learning/reconstruction/build_training_process),
  modifying `dataset_split_fn` to customize process behavior.

<div align="center">
  <img src="https://tensorflow.org/images/SIGAddons.png" width="60%"><br><br>
</div>

---

# TensorFlow Addons

**TensorFlow Addons** is a repository of contributions that conform to
well-established API patterns, but implement new functionality
not available in core TensorFlow. TensorFlow natively supports
a large number of operators, layers, metrics, losses, and optimizers.
However, in a fast moving field like ML, there are many interesting new
developments that cannot be integrated into core TensorFlow
(because their broad applicability is not yet clear, or it is mostly
used by a smaller subset of the community).

## Installation

#### Stable Builds

To install the latest version, run the following:

```
pip install tensorflow-addons
```

To use addons:

```python
import tensorflow as tf
import tensorflow_addons as tfa
```

#### Nightly Builds

There are also nightly builds of TensorFlow Addons under the pip package
`tfa-nightly`, which is built against the latest stable version of TensorFlow. Nightly builds
include newer features, but may be less stable than the versioned releases.

```
pip install tfa-nightly
```

#### Installing from Source

You can also install from source. This requires the [Bazel](https://bazel.build/) build system.

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

## Core Concepts

#### Standardized API within Subpackages

User experience and project maintainability are core concepts in
TF-Addons. In order to achieve these we require that our additions
conform to established API patterns seen in core TensorFlow.

#### GPU/CPU Custom-Ops

A major benefit of TensorFlow Addons is that there are precompiled ops. Should
a CUDA 10 installation not be found then the op will automatically fall back to
a CPU implementation.

#### Proxy Maintainership

Addons has been designed to compartmentalize subpackages and submodules so
that they can be maintained by users who have expertise and a vested interest
in that component.

Subpackage maintainership will only be granted after substantial contribution
has been made in order to limit the number of users with write permission.
Contributions can come in the form of issue closings, bug fixes, documentation,
new code, or optimizing existing code. Submodule maintainership can be granted
with a lower barrier for entry as this will not include write permissions to
the repo.

For more information see [the RFC](https://github.com/tensorflow/community/blob/master/rfcs/20190308-addons-proxy-maintainership.md)
on this topic.

#### Periodic Evaluation of Subpackages

Given the nature of this repository, subpackages and submodules may become less
and less useful to the community as time goes on. In order to keep the
repository sustainable, we'll be performing bi-annual reviews of our code to
ensure everything still belongs within the repo. Contributing factors to this
review will be:

1. Number of active maintainers
2. Amount of OSS use
3. Amount of issues or bugs attributed to the code
4. If a better solution is now available

Functionality within TensorFlow Addons can be categorized into three groups:

- **Suggested**: well-maintained API; use is encouraged.
- **Discouraged**: a better alternative is available; the API is kept for
  historic reasons; or the API requires maintenance and is the waiting period
  to be deprecated.
- **Deprecated**: use at your own risk; subject to be deleted.

The status change between these three groups is:
Suggested <-> Discouraged -> Deprecated.

The period between an API being marked as deprecated and being deleted will be
90 days. The rationale being:

1. In the event that TensorFlow Addons releases monthly, there will be 2-3
   releases before an API is deleted. The release notes could give user enough
   warning.

2. 90 days gives maintainers ample time to fix their code.

## Contributing

TF-Addons is a community led open source project. As such, the project
depends on public contributions, bug-fixes, and documentation. Please see
[contribution guidelines](https://github.com/tensorflow/addons/blob/master/CONTRIBUTING.md)
for a guide on how to contribute. This project adheres to [TensorFlow's code of conduct](https://github.com/tensorflow/addons/blob/master/CODE_OF_CONDUCT.md).
By participating, you are expected to uphold this code.

## Community

- [Public Mailing List](https://groups.google.com/a/tensorflow.org/forum/#!forum/addons)
- [SIG Monthly Meeting Notes](https://docs.google.com/document/d/1kxg5xIHWLY7EMdOJCdSGgaPu27a9YKpupUz2VTXqTJg)
  - Join our mailing list and receive calendar invites to the meeting

## License

[Apache License 2.0](LICENSE)

## Tables

Tables are supported with standard markup. Here's a typical table with a
heading row, several regular rows, and a row marked up as `<tr class="alt">`,
which produces a darker background that can be used as an alternate header.

| One                                    | Two | Three |
| -------------------------------------- | --- | ----- |
| 1.0                                    | 2.0 | 3.0   |
| 1.1                                    | 2.1 | 3.1   |
| Here come some numbers that end in .2! |     |       |
| 1.2                                    | 2.2 | 3.2   |

    <table>
      <tr><th>One</th><th>Two</th><th>Three</th></tr>
      <tr><td>1.0</td><td>2.0</td><td>3.0</td></tr>
      <tr><td>1.1</td><td>2.1</td><td>3.1</td></tr>
      <tr class="alt"><td colspan="3">Here come some numbers that end in .2!</td></tr>
      <tr><td>1.2</td><td>2.2</td><td>3.2</td></tr>
    </table>

### Responsive tables

To make a table responsive, add the `responsive` class to the table.

| Parameters       |                                                                                     |
| ---------------- | ----------------------------------------------------------------------------------- |
| `value`          | `String` the choice's value, which respondents see as a label when viewing the form |
| `navigationType` | `PageNavigationType` the choice's navigation type                                   |

- There must be **_only two columns_** in the table: the first column for the things being defined (the key), and the second column for all information about that key, in multiple lines if necessary. This two-column restriction means that responsive tables cannot be used for truly two-dimensional tabular data, checkmark-based feature comparison, but they are well suited for reference information (or anything other data that could reasonably be expressed by a definition list instead of a table).
- If there are multiple lines of information about the key — say, a type and a description — wrap each line in `<p>` to force line breaks (instead of `<br>`).
- There must be only one cell in the header row. Use `<th colspan="2">` to force it to span both columns. To remind you of this behavior, we automatically hides any `<th>` after the first (which intentionally looks very broken).

<!---->

    <table class="responsive">
      <tbody>
        <tr>
          <th colspan=2>Parameters</th>
        </tr>
        <tr>
          <td>
            <code>value</code>
          </td>
          <td>
            <code>String</code><br>
            the choice's value, which respondents see as a label when viewing the form
          </td>
        </tr>
        <tr>
          <td>
            <code>navigationType</code>
          </td>
          <td>
            <code>
              <a href="#">PageNavigationType</a>
            </code>
            <br>the choice's navigation type
          </td>
        </tr>
      </tbody>
    </table>

### Invisible tables

You can arrange text in columns, or otherwise make a table invisible, using
`<table class="columns">...</table>`. This is typically used for arranging
long narrow lists.

|                              |                                   |                                 |
| ---------------------------- | --------------------------------- | ------------------------------- |
| `auto` `break` `case` `char` | `const` `continue` `default` `do` | `double` `else` `enum` `extern` |

    <table class="columns">
      <tr>
        <td>
          <code>auto</code><br />
          <code>break</code><br />
          <code>case</code><br />
          <code>char</code>
        </td>
        <td>
          <code>const</code><br />
          <code>continue</code><br />
          <code>default</code><br />
          <code>do</code>
        </td>
        <td>
          <code>double</code><br />
          <code>else</code><br />
          <code>enum</code><br />
          <code>extern</code>
        </td>
      </tr>
    </table>

    <!--

This file is imported from https://github.com/ampproject/amphtml/blob/main/extensions/amp-a4a/amp-a4a-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

_If you'd like to propose changes to the standard, please comment on the [Intent
to Implement](https://github.com/ampproject/amphtml/issues/4264)_.

AMPHTML ads is a mechanism for rendering fast,
performant ads in AMP pages. To ensure that AMPHTML ad documents ("AMP
creatives") can be rendered quickly and smoothly in the browser and do
not degrade user experience, AMP creatives must obey a set of validation
rules. Similar in spirit to the
[AMP format rules](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml), AMPHTML ads have
access to a limited set of allowed tags, capabilities, and extensions.

## AMPHTML ad format rules <a name="amphtml-ad-format-rules"></a>

Unless otherwise specified below, the creative must obey all rules given by the
[AMP format rules](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml.html),
included here by reference. For example, the AMPHTML ad [Boilerplate](#boilerplate) deviates from the AMP standard boilerplate.

In addition, creatives must obey the following rules:

<table>
<thead>
<tr>
  <th>Rule</th>
  <th>Rationale</th>
</tr>
</thead>
<tbody>
<tr>
<td>Must use <code>&lt;html ⚡4ads></code> or <code>&lt;html amp4ads></code> as its enclosing tags.</td>
<td>Allows validators to identify a creative document as either a general AMP doc or a restricted AMPHTML ad doc and to dispatch appropriately.</td>
</tr>
<tr>
<td>Must include <code>&lt;script async src="https://cdn.ampproject.org/amp4ads-v0.js">&lt;/script></code> as the runtime script instead of <code>https://cdn.ampproject.org/v0.js</code>.</td>
<td>Allows tailored runtime behaviors for AMPHTML ads served in cross-origin iframes.</td>
</tr>
<tr>
<td>Must not include a <code>&lt;link rel="canonical"></code> tag.</td>
<td>Ad creatives don't have a "non-AMP canonical version" and won't be independently search-indexed, so self-referencing would be useless.</td>
</tr>
<tr>
<td>Can include optional meta tags in HTML head as identifiers, in the format of <code>&lt;meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"></code>. Those meta tags must be placed before the <code>amp4ads-v0.js</code> script. The value of <code>vendor</code> and <code>id</code> are strings containing only [0-9a-zA-Z_-]. The value of <code>type</code> is either <code>creative-id</code> or <code>impression-id</code>.</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p><pre>
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474">
&lt;meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"></pre></td>
</tr>
<tr>
<td><code>&lt;amp-analytics></code> viewability tracking may only target the full-ad selector, via  <code>"visibilitySpec": { "selector": "amp-ad" }</code> as defined in <a href="https://github.com/ampproject/amphtml/issues/4018">Issue #4018</a> and <a href="https://github.com/ampproject/amphtml/pull/4368">PR #4368</a>. In particular, it may not target any selectors for elements within the ad creative.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br>
<p>Example:</p>
<pre>
&lt;amp-analytics id="nestedAnalytics">
  &lt;script type="application/json">
  {
    "requests": {
      "visibility": "https://example.com/nestedAmpAnalytics"
    },
    "triggers": {
      "visibilitySpec": {
      "selector": "amp-ad",
      "visiblePercentageMin": 50,
      "continuousTimeMin": 1000
      }
    }
  }
  &lt;/script>
&lt;/amp-analytics>
</pre>
<p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p>
</td>
</tr>
</tbody>
</table>

### Boilerplate <a name="boilerplate"></a>

AMPHTML ad creatives require a different, and considerably simpler, boilerplate style line than [general AMP documents do](https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-boilerplate.md):

_Rationale:_ The `amp-boilerplate` style hides body content until the AMP
runtime is ready and can unhide it. If Javascript is disabled or the AMP
runtime fails to load, the default boilerplate ensures that the content is
eventually displayed regardless. In AMPHTML ads, however, if Javascript is entirely
disabled, AMPHTML ads won't run and no ad will ever be shown, so there is no need for
the `<noscript>` section. In the absence of the AMP runtime, most of the
machinery that AMPHTML ads rely on (e.g., analytics for visibility
tracking or `amp-img` for content display) won't be available, so it's better to
display no ad than a malfunctioning one.

Finally, the AMPHTML ad boilerplate uses `amp-a4a-boilerplate` rather than
`amp-boilerplate` so that validators can easily identify it and produce
more accurate error messages to help developers.

Note that the same rules about mutations to the boilerplate text apply as in
the [general AMP boilerplate](https://github.com/ampproject/amphtml/blob/main/docs/spec/amp-boilerplate.md).

### CSS <a name="css"></a>

<table>
<thead>
<tr>
  <th>Rule</th>
  <th>Rationale</th>
</tr>
</thead>
<tbody>
  <tr>
    <td><code>position:fixed</code> and <code>position:sticky</code> are prohibited in creative CSS.</td>
    <td><code>position:fixed</code> breaks out of shadow DOM, which AMPHTML ads depend on. lso, ads in AMP are already not allowed to use fixed position.</td>
  </tr>
  <tr>
    <td><code>touch-action</code> is prohibited.</td>
    <td>An ad that can manipulate <code>touch-action</code> can interfere with
   the user's ability to scroll the host document.</td>
  </tr>
  <tr>
    <td>Creative CSS is limited to 20,000 bytes.</td>
    <td>Large CSS blocks bloat the creative, increase network
   latency, and degrade page performance.
</td>
  </tr>
  <tr>
    <td>Transition and animation are subject to additional restrictions.</td>
    <td>AMP must be able to control all animations belonging to an
   ad, so that it can stop them when the ad is not on screen or system resources are very low.</td>
  </tr>
  <tr>
    <td>Vendor-specific prefixes are considered aliases for the same symbol
   without the prefix for the purposes of validation.  This means that if
   a symbol <code>foo</code> is prohibited by CSS validation rules, then the symbol <code>-vendor-foo</code> will also be prohibited.</td>
    <td>Some vendor-prefixed properties provide equivalent functionality to properties that are otherwise prohibited or constrained under these rules.<br><br><p>Example: <code>-webkit-transition</code> and <code>-moz-transition</code> are both considered aliases for <code>transition</code>.  They will only be allowed in contexts where bare <code>transition</code> would be allowed (see <a href="#selectors">Selectors</a> section below).</p></td>
  </tr>
</tbody>
</table>

#### CSS animations and transitions <a name="css-animations-and-transitions"></a>

##### Selectors <a name="selectors"></a>

The `transition` and `animation` properties are only allowed on selectors that:

- Contain only `transition`, `animation`, `transform`, `visibility`, or
  `opacity` properties.

  _Rationale:_ This allows the AMP runtime to remove this class from context
  to deactivate animations, when necessary for page performance.

##### Transitionable and animatable properties <a name="transitionable-and-animatable-properties"></a>

The only properties that may be transitioned are opacity and transform.
([Rationale](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/))

### Allowed AMP extensions and builtins <a name="allowed-amp-extensions-and-builtins"></a>

The following are _allowed_ AMP extension modules and AMP built-in tags in an
AMPHTML ad creative. Extensions or builtin tags not explicitly listed are prohibited.

- [amp-accordion](https://amp.dev/documentation/components/amp-accordion)
- [amp-ad-exit](https://amp.dev/documentation/components/amp-ad-exit)
- [amp-analytics](https://amp.dev/documentation/components/amp-analytics)
- [amp-anim](https://amp.dev/documentation/components/amp-anim)
- [amp-animation](https://amp.dev/documentation/components/amp-animation)
- [amp-audio](https://amp.dev/documentation/components/amp-audio)
- [amp-bind](https://amp.dev/documentation/components/amp-bind)
- [amp-carousel](https://amp.dev/documentation/components/amp-carousel)
- [amp-fit-text](https://amp.dev/documentation/components/amp-fit-text)
- [amp-font](https://amp.dev/documentation/components/amp-font)
- [amp-form](https://amp.dev/documentation/components/amp-form)
- [amp-img](https://amp.dev/documentation/components/amp-img)
- [amp-layout](https://amp.dev/documentation/components/amp-layout)
- [amp-lightbox](https://amp.dev/documentation/components/amp-lightbox)
- amp-mraid, on an experimental basis. If you're considering using this, please open an issue at [wg-monetization](https://github.com/ampproject/wg-monetization/issues/new).
- [amp-mustache](https://amp.dev/documentation/components/amp-mustache)
- [amp-pixel](https://amp.dev/documentation/components/amp-pixel)
- [amp-position-observer](https://amp.dev/documentation/components/amp-position-observer)
- [amp-selector](https://amp.dev/documentation/components/amp-selector)
- [amp-social-share](https://amp.dev/documentation/components/amp-social-share)
- [amp-video](https://amp.dev/documentation/components/amp-video)

Most of the omissions are either for performance or to make AMPHTML ads
simpler to analyze.

_Example:_ `<amp-ad>` is omitted from this list. It is explicitly disallowed
because allowing an `<amp-ad>` inside an `<amp-ad>` could potentially lead to
unbounded waterfalls of ad loading, which does not meet AMPHTML ads performance goals.

_Example:_ `<amp-iframe>` is omitted from this list. It is disallowed
because ads could use it to execute arbitrary Javascript and load arbitrary
content. Ads wanting to use such capabilities should return `false` from
their
[a4aRegistry](https://github.com/ampproject/amphtml/blob/main/ads/_a4a-config.js#L40)
entry and use the existing '3p iframe' ad rendering mechanism.

_Example:_ `<amp-facebook>`, `<amp-instagram>`, `<amp-twitter>`, and
`<amp-youtube>` are all omitted for the same reason as `<amp-iframe>`: They
all create iframes and can potentially consume unbounded resources in them.

_Example:_ `<amp-ad-network-*-impl>` are omitted from this list. The
`<amp-ad>` tag handles delegation to these implementation tags; creatives
should not attempt to include them directly.

_Example:_ `<amp-lightbox>` is not yet included because even some AMPHTML ads creatives
may be rendered in an iframe and there is currently no mechanism for an ad to
expand beyond an iframe. Support may be added for this in the future, if there
is demonstrated desire for it.

### HTML tags <a name="html-tags"></a>

The following are _allowed_ tags in an AMPHTML ads creative. Tags not explicitly
allowed are prohibited. This list is a subset of the general [AMP tag
addendum allowlist](https://github.com/ampproject/amphtml/blob/main/extensions/amp-a4a/../../spec/amp-tag-addendum.md). Like that list, it is
ordered consistent with HTML5 spec in section 4 [The Elements of HTML](http://www.w3.org/TR/html5/single-page.html#html-elements).

Most of the omissions are either for performance or because the tags are not
HTML5 standard. For example, `<noscript>` is omitted because AMPHTML ads depends on
JavaScript being enabled, so a `<noscript>` block will never execute and,
therefore, will only bloat the creative and cost bandwidth and latency.
Similarly, `<acronym>`, `<big>`, et al. are prohibited because they are not
HTML5 compatible.

#### 4.1 The root element <a name="41-the-root-element"></a>

4.1.1 `<html>`

- Must use types `<html ⚡4ads>` or `<html amp4ads>`

#### 4.2 Document metadata <a name="42-document-metadata"></a>

4.2.1 `<head>`

4.2.2 `<title>`

4.2.4 `<link>`

- `<link rel=...>` tags are disallowed, except for `<link rel=stylesheet>`.
- **Note:** Unlike in general AMP, `<link rel="canonical">` tags are
  prohibited.

  4.2.5 `<style>`
  4.2.6 `<meta>`

#### 4.3 Sections <a name="43-sections"></a>

4.3.1 `<body>`
4.3.2 `<article>`
4.3.3 `<section>`
4.3.4 `<nav>`
4.3.5 `<aside>`
4.3.6 `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, and `<h6>`
4.3.7 `<header>`
4.3.8 `<footer>`
4.3.9 `<address>`

#### 4.4 Grouping Content <a name="44-grouping-content"></a>

4.4.1 `<p>`
4.4.2 `<hr>`
4.4.3 `<pre>`
4.4.4 `<blockquote>`
4.4.5 `<ol>`
4.4.6 `<ul>`
4.4.7 `<li>`
4.4.8 `<dl>`
4.4.9 `<dt>`
4.4.10 `<dd>`
4.4.11 `<figure>`
4.4.12 `<figcaption>`
4.4.13 `<div>`
4.4.14 `<main>`

#### 4.5 Text-level semantics <a name="45-text-level-semantics"></a>

4.5.1 `<a>`
4.5.2 `<em>`
4.5.3 `<strong>`
4.5.4 `<small>`
4.5.5 `<s>`
4.5.6 `<cite>`
4.5.7 `<q>`
4.5.8 `<dfn>`
4.5.9 `<abbr>`
4.5.10 `<data>`
4.5.11 `<time>`
4.5.12 `<code>`
4.5.13 `<var>`
4.5.14 `<samp>`
4.5.15 `<kbd >`
4.5.16 `<sub>` and `<sup>`
4.5.17 `<i>`
4.5.18 `<b>`
4.5.19 `<u>`
4.5.20 `<mark>`
4.5.21 `<ruby>`
4.5.22 `<rb>`
4.5.23 `<rt>`
4.5.24 `<rtc>`
4.5.25 `<rp>`
4.5.26 `<bdi>`
4.5.27 `<bdo>`
4.5.28 `<span>`
4.5.29 `<br>`
4.5.30 `<wbr>`

#### 4.6 Edits <a name="46-edits"></a>

4.6.1 `<ins>`
4.6.2 `<del>`

#### 4.7 Embedded Content <a name="47-embedded-content"></a>

- Embedded content is supported only via AMP tags, such as `<amp-img>` or
  `<amp-video>`.

#### 4.7.4 `<source>` <a name="474-source"></a>

#### 4.7.18 SVG <a name="4718-svg"></a>

SVG tags are not in the HTML5 namespace. They are listed below without section ids.

`<svg>`
`<g>`
`<path>`
`<glyph>`
`<glyphref>`
`<marker>`
`<view>`
`<circle>`
`<line>`
`<polygon>`
`<polyline>`
`<rect>`
`<text>`
`<textpath>`
`<tref>`
`<tspan>`
`<clippath>`
`<filter>`
`<lineargradient>`
`<radialgradient>`
`<mask>`
`<pattern>`
`<vkern>`
`<hkern>`
`<defs>`
`<use>`
`<symbol>`
`<desc>`
`<title>`

#### 4.9 Tabular data <a name="49-tabular-data"></a>

4.9.1 `<table>`
4.9.2 `<caption>`
4.9.3 `<colgroup>`
4.9.4 `<col>`
4.9.5 `<tbody>`
4.9.6 `<thead>`
4.9.7 `<tfoot>`
4.9.8 `<tr>`
4.9.9 `<td>`
4.9.10 `<th>`

#### 4.10 Forms <a name="410-forms"></a>

4.10.8 `<button>`

#### 4.11 Scripting <a name="411-scripting"></a>

- Like a general AMP document, the creative's `<head>` tag must contain a
  `<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>` tag.
- Unlike general AMP, `<noscript>` is prohibited.
- _Rationale:_ Since AMPHTML ads requires Javascript to be enabled to function
  at all, `<noscript>` blocks serve no purpose in AMPHTML ads and
  only cost network bandwidth.
- Unlike general AMP, `<script type="application/ld+json">` is
  prohibited.
- _Rationale:_ JSON LD is used for structured data markup on host
  pages, but ad creatives are not standalone documents and don't
  contain structured data. JSON LD blocks in them would just cost
  network bandwidth.
- All other scripting rules and exclusions are carried over from general
  AMP.

<span attr="some attr">Or use the </span> hotkey (<kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>M</kbd> by default).

<customtag attr="some attr">Text </customtag>

See <div>view </div>

<div>View </div>

<div align="center">
  <img src="https://tensorflow.org/images/SIGAddons.png" width="60%"><br><br>
</div>

First and <main attr="attr">second</main> after
