**[API docs](http://ptaoussanis.github.io/touchstone/)** | **[CHANGELOG](https://github.com/ptaoussanis/touchstone/blob/master/CHANGELOG.md)** | [contact & contributing](#contact--contributing) | [other Clojure libs](https://www.taoensso.com/clojure-libraries) | [Twitter](https://twitter.com/#!/ptaoussanis) | current [semantic](http://semver.org/) version:

```clojure
[com.taoensso/touchstone "2.0.0-RC2"] ; Development (notes below)
[com.taoensso/touchstone "1.0.0"]     ; Stable, needs Clojure 1.4+ as of 1.0.0
```

v2 is a **BREAKING** release. It adds explicit test config and test-subject id args to every relevant API fn. See the [CHANGELOG](https://github.com/ptaoussanis/touchstone/blob/master/CHANGELOG.md) for migration details.

# Touchstone, a Clojure A/B testing library

[A/B testing](http://en.wikipedia.org/wiki/A/B_testing) is great for **conversion optimization**. We should all be doing more of it. But traditional A/B tests can be a nuisance to setup and monitor.

Touchstone is an attempt to bring **dead-simple, high-power split-testing** to any Clojure web application. It uses [multi-armed bandit](http://en.wikipedia.org/wiki/Multi-armed_bandit) techniques to provide **fast, accurate, low-maintenance** conversion optimization. The API is very simple and *highly flexible*.

## What's in the box™?
 * Small, uncomplicated **all-Clojure** library.
 * **Great performance** (backed by [Carmine](https://github.com/ptaoussanis/carmine)).
 * **High flexibility** (variations are *arbitrary Clojure forms*).
 * **Low maintenace** (fire-and-forget, automatic-selection algorithm).
 * Fire-and-forget **multivariate** testing.
 * **Advanced capabilities** like test composition (dependent tests), arbitrary scoring, engagement testing, etc.
 * **Ring middleware**.

## Getting started

### Dependencies

Add the necessary dependency to your [Leiningen](http://leiningen.org/) `project.clj` and `require` the library in your ns:

```clojure
[com.taoensso/touchstone "2.0.0-RC2"] ; project.clj
(ns my-app (:require [taoensso.touchstone :as touchstone :refer (*ts-id*)])) ; ns
```

### Split-testing

Traditional split-testing consists of 4 steps:
  1. Defining content variations (e.g. possible labels for a sign-up button).
  2. Distributing content variations to test subjects (our users).
  3. Recording events of interest (sign-ups) by variation.
  4. Analyzing the results and adopting our most successful content (best button label).

The particular multi-armed bandit technique used by Touchstone means that we only concern ourselves with steps 1 and 3. Steps 2 and 4 are handled automatically by the algorithm.

**To optimize a Ring web application**, start by adding `(taoensso.touchstone.ring/wrap-random-subject-id)` to your middleware stack.

One or more test selectors can then be used as part of your page content:

```clojure
(touchstone/mab-select {} *ts-id* :my-app/landing.buttons.sign-up ; Test id
                       :sign-up  "Sign-up!"   ; Named variation #1
                       :join     "Join!"      ; Named variation #2
                       :join-now "Join now!"  ; Named variation #3
                       )
```

And relevant events (e.g. conversions) recorded:

```clojure
(touchstone/mab-commit! {} *ts-id* :my-app/landing.buttons.sign-up 1) ; On sign-up button click
```

Touchstone will now **automatically** start using accumulated statistical data to optimize the selection of the `:my-app/landing.buttons.signup` test variations for maximum clicks.

And you're done! That's literally all there is to it.

See the `mab-select` and `mab-commit!` docstrings for info on more advanced capabilities like **multivariate testing, test composition (dependent tests), arbitrary scoring, engagement testing**, etc.

## This project supports the CDS and ![ClojureWerkz](https://raw.github.com/clojurewerkz/clojurewerkz.org/master/assets/images/logos/clojurewerkz_long_h_50.png) goals

  * [CDS](http://clojure-doc.org/), the **Clojure Documentation Site**, is a **contributer-friendly** community project aimed at producing top-notch, **beginner-friendly** Clojure tutorials and documentation. Awesome resource.

  * [ClojureWerkz](http://clojurewerkz.org/) is a growing collection of open-source, **batteries-included Clojure libraries** that emphasise modern targets, great documentation, and thorough testing. They've got a ton of great stuff, check 'em out!

## Contact & contribution

Please use the [project's GitHub issues page](https://github.com/ptaoussanis/touchstone/issues) for project questions/comments/suggestions/whatever **(pull requests welcome!)**. Am very open to ideas if you have any!

Otherwise reach me (Peter Taoussanis) at [taoensso.com](https://www.taoensso.com) or on Twitter ([@ptaoussanis](https://twitter.com/#!/ptaoussanis)). Cheers!

## License

Copyright &copy; 2012, 2013 Peter Taoussanis. Distributed under the [Eclipse Public License](http://www.eclipse.org/legal/epl-v10.html), the same as Clojure.
