

Date: 16/06/2026
Tags: [[-Java]]

# Notes

Nous parlons de **shadowing** lorsqu'un attribut porte le même nom qu'un argument.

**Ceci est du shadowing** 
```java
private String name;  

public User(String name) {  
	// le this est l'adresse mémoire de l'instance
    this.name = name;  
}
```

**Ceci n'est pas du shadowing**
```java
private String name;  

public User(String nameUser) {  
	// ici, le this est optionnel
    this.name = nameUser;  
}
```

# Réf

Stéphane Gobin

