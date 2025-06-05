# SCAS (Smile Computer Algebra System)

SCAS is a lightweight, purely functional symbolic algebra library in Scala, originally forked from the open-source Smile CAS module. It has been restructured to work simply with scala-cli, allowing you to experiment with symbolic expressions, differentiation, expansion, and factoring using a simple REPL.

---

## Table of Contents
- [Features](#features)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Example: Differentiation](#example-differentiation)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

---

## Features
- **Symbolic Variables & Constants**  
  Create symbolic variables (`Var`) and constants (`Val`, `Const`), including integer analogues (`IntVar`, `IntVal`, `IntConst`).

- **Basic Operations**  
  Support for `+`, `-`, `*`, `/`, `**` (power), unary `-`, and a variety of built-in functions (`sin`, `cos`, `exp`, `log`, etc.).

- **Algebraic Simplification**  
  Automatic simplification rules (e.g., `0 + x = x`, `x * 1 = x`, `x - x = 0`, constant folding, and some trigonometric/log identities).

- **Expansion & Factoring**  
  - `expand` will distribute products and binomial powers (e.g., `(x + 2)^3 → x^3 + 6*x^2 + 12*x + 8`).  
  - `factor` will attempt to identify perfect squares of the form `x^2 + 2·b·x + b^2 → (x + b)^2`.

- **Differentiation**  
  - `d(dx: Var): Scalar` computes the derivative with respect to a single symbolic variable.  
  - `d(dx: VectorVar): Vector` computes a gradient vector (for multivariate cases).  
  - Chain rule is implemented via the `SymFunc` case class for arbitrary symbolic functions.

- **Purely Functional Design**  
  No mutable state—every transformation returns a new `Scalar` or `Vector` instance.

---

## Getting Started

1. **Clone this repository**  
   ```bash
   git clone https://your-git-server.com/your-username/scas.git
   cd scas
   ```

2. **Launch a Scala REPL**  
   From the `scas/` directory, run:
   ```bash
   scala-cli repl .
   ```
   This will start a REPL session with all source files under `src/` on the classpath. You can then import and work with the CAS API interactively.

---

## Usage

Once inside the REPL, you can import the top-level package and begin defining symbolic expressions:

```scala
scala> import smile.cas._
```

- **Create Variables**  
  ```scala
  scala> val x = Var("x")
  scala> val y = Var("y")
  ```

- **Build Expressions**  
  ```scala
  scala> val expr1: Scalar = (x + Val(2)) ** Val(3)           // (x + 2)**3
  scala> val expr2: Scalar = Sin(x) * Exp(x) + x ** Val(2)    // sin(x)*e(x) + x**2
  ```

- **Evaluate / Apply**  
  ```scala
  scala> expr1(Map("x" -> Val(5)))   // substitutes x = 5
  val res0: Scalar = 343.0
  ```

- **Simplify / Expand / Factor**  
  ```scala
  scala> expr1.expand
  val res1: Scalar = x**3.0 + 6.0*(x**2.0) + 12.0*x + 8.0

  scala> (x**Val(2) + Val(2)*x + Val(1)).factor
  val res2: Scalar = (x + 1.0)**2.0
  ```

- **Differentiate**  
  ```scala
  scala> expr2.d(x).simplify
  val res3: Scalar = (sin(x) + cos(x) * x)*e(x) + 2.0*x
  ```
  (Here `e(x)` denotes `exp(x)` under the hood.)

---

## Project Structure

```
scas/
├── README.md
├── project.scala
├── LICENSE               # GPLv3 as inherited from Smile (see below)
└── src
    └── smile
        └── cas
            ├── Scalar.scala
            ├── Vector.scala
            ├── Matrix.scala
            ├── Tensor.scala
            ├── … (all other case classes and traits)
            └── combinations (private helper)
```

> **Note:** All code lives under `src/smile/cas`. When you run `scala-cli repl .` from the `scas/` folder, scala-cli automatically picks up the entire `src/` directory so you can import `smile.cas._` directly.

---

## Contributing
1. Fork this repository.
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Commit your changes.
4. Push to your branch:
   ```bash
   git push origin feature/your-feature-name
   ```
5. Open a Pull Request with a clear description of your changes.

Please follow the existing style:
- Purely functional implementations.
- No mutable state.
- Consistent naming conventions (CamelCase for types, lowerCamelCase for methods).
- Adequate unit tests (e.g., using uTest or ScalaTest).

---

## License

This project is derived from the Smile CAS module, which is licensed under the GNU General Public License v3.0 (GPL-3.0). All modifications made by Daniel Meyer in 2025 retain that license.

See the `LICENSE` file for full details.

---

Happy Symbolic Algebra!