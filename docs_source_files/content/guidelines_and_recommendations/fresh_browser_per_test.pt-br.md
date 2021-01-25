---
title: "Navegador novo por teste"
weight: 9
---

Comece cada teste a partir de um estado limpo conhecido.
Idealmente, ligue uma nova máquina virtual para cada teste.
Se ligar uma nova máquina virtual não for prático,
pelo menos inicie um novo WebDriver para cada teste.
Para Firefox, inicie um WebDriver com seu perfil conhecido.

```java
FirefoxProfile profile = new FirefoxProfile(new File("pathToFirefoxProfile"));
WebDriver driver = new FirefoxDriver(profile);
```
