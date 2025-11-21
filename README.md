# ***Exposé Model De Conception***
## *Pattern en Facade*


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
le système est trop facile.

besoin d’avoir un accès direct à certaines fonctionnalités. 
besoin de faire des changements internes profonds.


## 7. Cas d’Usage Réalisé


*Source :*

https://www.baeldung.com/java-facade-pattern

https://www.baeldung.com/java-visitor-pattern

https://www.ionos.fr/digitalguide/sites-internet/developpement-web/quest-ce-quun-facade-pattern/#:~:text=Le%20patron%20de%20façade%20est,Reusable%20Object-Oriented%20Software%20».

https://refactoring.guru/fr/design-patterns/facade
