# ***Exposé Model De Conception***
# *Pattern en Facade*


## 1. Introduction / Définition
Le pattern de conception Facade est un modèle structurel (pattron) utilisé afin d'unifier l'utilisation d'interfaces multiples d'un sous système plus complexe.

Ça répond à la difficulté de manipuler un logiciel peu lisible doté de plusieurs classes et de dépendances.
Il sert de "filtre" entre le code utilisateur et les interfaces complexes pour rendre l'utilisation, l'encapsulation et la maintenance du code plus facile à gèrer en créant une "couche intermédiaire" appelé Facade.

## 2. Contexte d’Utilisation
Le pattern de conception Facade s'utilise pour différentes fonctions :
1. Complexité masquée : Lorsqu’un sous-système est composé de nombreuses classes avec des dépendances complexes, et que le client n’a pas besoin de connaître ces détails.
2. Couplage réduit : Pour éviter que le code client soit fortement couplé à plusieurs classes du sous-système, ce qui rendrait la maintenance plus difficile.
3. Simplification d’API : Lorsqu’une bibliothèque ou un framework expose une API trop complexe pour une utilisation basique, une façade permet d'obtenir une interface plus     simple.
4. Amélioration de la lisibilité : Pour rendre le code client plus lisible et plus facile à comprendre en centralisant les appels aux sous-systèmes.

## 3. Structure du Pattern
<img width="1012" height="542" alt="image" src="https://github.com/user-attachments/assets/dcb46232-6f09-4d1e-9828-e582fa685c69" />

## 4. Exemple de Code
***Scénario :***

Un système de gestion de commande en ligne avec plusieurs étapes complexes (vérification de stock, paiement, livraison). La façade simplifie ces étapes pour le client.
```java
// Sous-système : Classes internes complexes
class StockService {
    public boolean checkStock(String productId) {
        System.out.println("Vérification du stock pour " + productId + "...");
        return true; // Simplification
    }
}

class PaymentService {
    public boolean processPayment(double amount) {
        System.out.println("Traitement du paiement de " + amount + "€...");
        return true; // Simplification
    }
}

class ShippingService {
    public boolean shipOrder(String productId) {
        System.out.println("Expédition du produit " + productId + "...");
        return true; // Simplification
    }
}

// Facade : Interface simplifiée
class OrderFacade {
    private StockService stock;
    private PaymentService payment;
    private ShippingService shipping;

    public OrderFacade() {
        this.stock = new StockService();
        this.payment = new PaymentService();
        this.shipping = new ShippingService();
    }

    public String placeOrder(String productId, double amount) {
        if (!stock.checkStock(productId)) {
            return "Échec : Stock insuffisant.";
        }

        if (!payment.processPayment(amount)) {
            return "Échec : Paiement refusé.";
        }

        if (!shipping.shipOrder(productId)) {
            return "Échec : Problème de livraison.";
        }

        return "Commande passée avec succès !";
    }
}

// Client : Utilisation simplifiée
public class Main {
    public static void main(String[] args) {
        OrderFacade facade = new OrderFacade();
        String result = facade.placeOrder("ABC123", 99.99);
        System.out.println(result);
    }
}
```


## 5. Avantages / Inconveniant
| Avantages | Inconvénients |
|---------------|-------------------|
| Simplifie l’interface et l'utilisation | Implémentation parfois complexe si le code existe déjà. |
| Réduit les dépendances entre les classes, ce qui facilite les modifications mineures du code. |  Risque de masquer des fonctionnalités importantes du sous-système si la façade est mal conçue.  |
| Encapsule le code, limitant les risques d’erreurs ou d’usages imprévus.  | Point de défaillance unique : si la façade dysfonctionne, tout le système est touché. |
| Facilite l’écriture de tests unitaires. | Peut devenir une classe surchargée ou « omnisciente » si trop de responsabilités y sont regroupées. |
| Cache la complexité technique pour aider les développeurs débutants. | Impact potentiel sur les performances à cause de la couche d’abstraction. |
### Mauvaise pratique
Il existe aussi différentes situations dans lesquelles il ne faut pas utiliser ce modèle : 

Le système est trop facile.

Besoin d’avoir un accès direct à certaines fonctionnalités. 

Besoin de faire des changements internes profonds.


# *Pattern avec Visiteur*

## 1. Introduction / Définition
Le pattern Visiteur est un patron de conception qui permet de :

Ajouter de nouvelles opérations sur une hiérarchie de classes sans les modifier, et de séparer la structure des données (les éléments) et les algorithmes qui s’y appliquent (les visiteurs)

## 2. Contexte d’Utilisation

Le Visiteur est utile quand on a une structure d’objets stable tel que des arbre syntaxique, document, produits, etc...) mais que les traitements à appliquer à ces objets évoluent souvent (export, rendu, statistiques, règles métier, etc.).​

Il est particulièrement adapté aux structures hiérarchiques complexes (arbres ou graphes) où l’on souhaite appliquer plusieurs opérations différentes sans mélanger la logique métier de chaque élément.

## 3. Structure du Pattern

<img width="1046" height="702" alt="azeeza" src="https://github.com/user-attachments/assets/68f1259f-bd3a-4b38-bbad-799ca934ceae" />

## 4. Exemple de Code

```java 
class Circle implements Shape {
    double radius;

    Circle(double radius) { this.radius = radius; }

    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visit(this); 
    }
}

class Square implements Shape {
    double side;

    Square(double side) { this.side = side; }

    @Override
    public void accept(ShapeVisitor visitor) {
        visitor.visit(this);
    }
}
// Visiteur qui calcule l'aire d'un cercle
class AreaVisitor implements ShapeVisitor {
    @Override
    public void visit(Circle c) {
        double area = Math.PI * c.radius * c.radius;
        System.out.println("Aire du cercle : " + area);
    }
//Visiteur qui calcule l'aire d'un carré
    @Override
    public void visit(Square s) {
        double area = s.side * s.side;
        System.out.println("Aire du carré : " + area);
    }
}

// 2) Un visiteur qui affiche les formes
class PrintVisitor implements ShapeVisitor {
    @Override
    public void visit(Circle c) {
        System.out.println("Cercle de rayon " + c.radius);
    }

    @Override
    public void visit(Square s) {
        System.out.println("Carré de côté " + s.side);
    }
} 
```

## 5. Avantages / Inconveniant

| Avantages | Inconvénients |
|-----------|----------------|
| Ajout d’un nouveau comportement pouvant accepter des objets de différentes classes sans les modifier. | Vous devez mettre à jour les visiteurs chaque fois qu’une classe est ajoutée ou retirée. |
| Possibilité de regrouper plusieurs comportements dans une seule classe (plusieurs versions d’un même comportement). | Les visiteurs n’ont parfois pas les accès nécessaires aux attributs ou méthodes privés. |
| Peut garder en mémoire des données lorsqu’il parcourt la structure. | Plus il y a de classes, plus il y a de visiteurs, ce qui peut rendre le code complexe. |
| Les objets conservent leurs données et les visiteurs contiennent les actions : pour modifier une action, il suffit de modifier le visiteur. | Peut violer les règles du principe d’encapsulation. |
| Pour ajouter une nouvelle opération, il suffit de créer un nouveau visiteur. | Lors de l’ajout de nouveaux types d’objets, il faut modifier le visiteur ou l’interface Visitor. |
| Facile à maintenir. |  |
| Le visiteur s’adapte au type de l’objet qu’il visite. |  |
| Chaque visiteur a son propre comportement, ce qui permet une bonne organisation du code. |  |

Il existe aussi différentes situations dans lesquelles il ne faut pas utiliser ce modèle :

Si tu dois souvent changer ta hiérarchie de classe.

Quand la structure des objets est complexe. 

Quand les opérations sont simples et peu nombreuses, elles vont complexifier le code pour rien.

*Source :*

https://www.baeldung.com/java-facade-pattern

https://www.baeldung.com/java-visitor-pattern

https://www.ionos.fr/digitalguide/sites-internet/developpement-web/quest-ce-quun-facade-pattern/#:~:text=Le%20patron%20de%20façade%20est,Reusable%20Object-Oriented%20Software%20».

https://refactoring.guru/fr/design-patterns/facade

https://refactoring.guru/fr/design-patterns/visitor 

https://www.sfeir.dev/back/les-design-patterns-comportementaux-visiteur/#:~:text=de%20données%20complexes-,Avantages,modifier%20les%20classes%20des%20objets.
