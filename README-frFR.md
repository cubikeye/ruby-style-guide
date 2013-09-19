# Préambule

> Le style est ce qui sépare le bon de l'excellent. <br/>
> -- Bozhidar Batsov

Une chose m'a toujours gêné en tant que développeur Ruby - les développeurs
Python ont une super référence de style de programmation
([PEP-8](http://www.python.org/dev/peps/pep-0008/)) alors que nous n'avons
jamais eu un tel guide officiel, définissant le style et les bonnes pratiques
de programmation Ruby. Et je pense que le style compte. Je pense également
que des gens biens, tels que nous, développeurs Ruby, devraient être
tout à fait capables de produire ce document convoité.

Ce guide (écrit par votre serviteur) a vu le jour en tant que convention de
programmation Ruby interne à notre entreprise. Au bout d'un certain temps,
il m'a semblé que ce travail pourrait s'avérer intéressant pour les membres
de la communauté ruby en général et que le monde n'avait pas vraiment besoin
d'une convention de programmation interne de plus. Mais le monde pourrait
certainement tirer profit d'un ensemble de pratiques, d'idiomes et de conseils
de style pour la programmation Ruby.

Dès le lancement de ce guide, j'ai reçu énormément de retours des membres de
l'exceptionnelle communauté Ruby à travers le monde. Merci pour toutes les
suggestions et votre soutien! Ensemble, nous pouvons créer une ressource
bénéfique à tous les développeurs Ruby ici présents.

Au fait, si vous faites du Rails, vous pouvez consulter le
[Guide de style Ruby on Rails 3](https://github.com/bbatsov/rails-style-guide) complémentaire.

# Le guide de style Ruby

Ce guide de style Ruby recommande des bonnes pratiques conçues pour qu'un
programmeur Ruby du monde réel puisse produire un code qui puisse être
maintenu par d'autres programmeurs Ruby du monde réel. Un guide de style qui
reflète les utilisations du monde réel est utilisé, et un guide de style qui
repose sur un idéal qui a été rejeté par les personnes qu'il est censé aider
a de bonnes chances de ne pas être utilisé du tout &ndash; peu importe sa qualité.

Le guide est divisé en plusieurs sections de règles connexes. J'ai essayé
d'ajouter le rationnel derrière les règles (j'ai omis certains points que je
considérais plutôt évidents).

Ces règles ne sont pas sorties de nulle part - la majeure partie est basée sur
ma longue carrière d'ingénieur logiciel, les retours et les suggestions des membres
de la communauté Ruby et diverses ressources très renommées sur la programmation
Ruby, telles que ["Programming Ruby 1.9"](http://pragprog.com/book/ruby3/programming-ruby-1-9)
et ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

Ce guide est toujours un travail en cours - certaines règles n'ont pas d'exemple,
d'autres ont des exemples qui ne les illustre pas assez clairement. Ces problèmes
seront corrigés tôt ou tard - gardez les juste à l'esprit pour l'instant.

Vous pouvez générer une copie PDF ou HTML de ce guide en utilisant
[Transmuter](https://github.com/TechnoGate/transmuter).

Le projet [rubocop](https://github.com/bbatsov/rubocop) vise à fournir un moyen
automatisé pour vérifier si un code Ruby est conforme avec le guide de style.
Pour l'instant, il est loin d'être prêt pour la production et il lui manque
beaucoup de fonctionnalités. Naturellement, tout le monde est invité à participer
à son amélioration!

Des traductions de ce guide sont disponibles dans les langues suivantes :

* [Anglais](https://github.com/bbatsov/ruby-style-guide/blob/master/README.md) (version originale)
* [Chinois simplifié](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhCN.md)
* [Chinois traditionnel](https://github.com/JuanitoFatas/ruby-style-guide/blob/master/README-zhTW.md)

## Table des matières

* [Structure du code source](#structure-du-code-source)
* [Syntaxe](#syntaxe)
* [Nommage](#nommage)
* [Commentaires](#commentaires)
    * [Commentaires d'annotation](#commentaires-dannotation)
* [Classes](#classes)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Chaînes de caractères](#chaines-de-caractères)
* [Expressions rationnelles](#expressions-rationnelles)
* [Notations pourcents](#notations-pourcents)
* [Méta-programmation](#méta-programmation)
* [Divers](#divers)

## Structure du code source

> Chacun de nous ou presque est convaincu que tous les styles sauf le notre sont
> moches et illisibles. Retirez "sauf le notre" et nous aurons probablement
> raison... <br/>
> -- Jerry Coffin (à propos de l'indentation)

* Utilisez l'`UTF-8` comme encodage des fichiers source.
* Utilisez deux **espaces** par niveau d'indentation. Pas de tabulation.

    ```Ruby
    # bien
    def some_method
      do_something
    end

    # mauvais - quatre espaces
    def some_method
        do_something
    end
    ```

* Utilisez des fins de ligne de type Unix. (*Les utilisateurs BSD/Solaris/Linux/OSX sont couverts par défaut,
  les utilisateurs Windows doivent faire très attention.)
    * Si vous utilisez Git, vous pouvez ajouter les paramètres de configuration
    suivants pour protéger votre projet des fins de ligne Windows qui traîneraient:

        $ git config --global core.autocrlf true

* Utilisez des espaces autour des opérateurs, après les deux points, points-virgules et virgules,
  autour de `{` et devant `}`. L'espace est (généralement) sans importance pour
  l'interpréteur Ruby, mais l'utiliser correctement permet d'écrire facilement du code lisible.

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    L'opérateur d'exposant est la seule exception:
    
    ```Ruby
    # mauvais
    e = M * c ** 2

    # bien
    e = M * c**2
    ```

* Pas d'espace après `(`, `[` ou devant `]`, `)`.

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* Indenter `when` au même niveau que `case`. Je sais que beaucoup
  seront en désaccord avec cette règle, mais c'est un style établi
  dans "The Ruby Programming Language" et "Programming Ruby".

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duration > 120
      puts 'Too long!'
    when Time.now.hour > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* Passez des lignes entre les `def` pour diviser la méthode en paragraphes
  logiques.

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_method
      result
    end
    ```

* Alignez les paramètres de l'appel de méthode s'ils s'étalent sur plus d'une ligne.

    ```Ruby
    # point de départ (ligne trop longue)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end

    # mauvais (indentation normale)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # mauvais (double indentation)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end

    # bien
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* Ajoutez des tirets bas aux grands expressions numériques pour améliorer leur lisibilité.

    ```Ruby
    # mauvais - combien y a-t-il de zéros?
    num = 1000000

    # bien - bien plus simple à analyser pour le cerveau humain
    num = 1_000_000
    ```

* Utilisez RDoc et ses conventions pour la documentation d'API. Ne
  passez pas de ligne entre le bloc de commentaire et le `def`.
* Limitez les lignes à 80 caractères.
* Evitez de laisser des espaces en fin de ligne.

## Syntaxe

* Utilisez `def` avec des parenthèses quand il y a des arguments.
  Omettez les parenthèses quand la méthode n'accepte pas d'argument.

     ```Ruby
     def some_method
       # body omitted
     end

     def some_method_with_arguments(arg1, arg2)
       # body omitted
     end
     ```

* N'utilisez jamais `for`, sauf si vous savez exactement pourquoi. En général,
  il vaut mieux utiliser les itérateurs. `for` est implémenté à la suite de `each`,
  mais avec une différence - `for` n'a pas de contexte propre (contrairement à `each`)
  et les variables définies dans son bloc seront visibles en dehors.

    ```Ruby
    arr = [1, 2, 3]

    # mauvais
    for elem in arr do
      puts elem
    end

    # bien
    arr.each { |elem| puts elem }
    ```

* N'utilisez jamais `then` pour les `if/unless` sur plusieurs lignes.

    ```Ruby
    # mauvais
    if some_condition then
      # body omitted
    end

    # bien
    if some_condition
      # body omitted
    end
    ```

* Privilégiez l'opérateur ternaire (`?:`) plutôt que des structures `if/then/else/end`.
  C'est plus courant et visiblement plus compact.

    ```Ruby
    # mauvais
    result = if some_condition then something else something_else end

    # bien
    result = some_condition ? something : something_else
    ```

* Utilisez une seule expression par section dans un opérateur ternaire.
  Autrement dit, les opérateurs ternaires ne doivent pas être
  imbriqués. Privilégiez les structures `if/else` le cas échéant.

    ```Ruby
    # mauvais
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # bien
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* N'utilisez jamais `if x: ...` - cette syntaxe est supprimée en Ruby 1.9.
  Utilisez l'opérateur ternaire à la place.

    ```Ruby
    # mauvais
    result = if some_condition: something else something_else end

    # bien
    result = some_condition ? something : something_else
    ```

* N'utilisez jamais `if x; ...`. Utilisez l'opérateur ternaire à la place.

* Utilisez `when x then ...` pour les `case` sur une seule ligne.
  La syntaxe alternative `when x: ...` est supprimée en Ruby 1.9.

* N'utilisez jamais `when x; ...`. Voir règle précédente.

* Utilisez `&&/||` pour les expressions booléennes, `and/or` pour le flux de contrôle.
  (Règle d'or: Si vous devez mettre des parenthèses exterieures, vous utilisez les
  mauvais opérateurs.)

    ```Ruby
    # expression booléenne
    if some_condition && some_other_condition
      do_something
    end

    # flux de contrôle
    document.saved? or document.save!
    ```

* Evitez les `?:` sur plusieurs lignes (opérateur ternaire); utilisez `if/unless` à la place.

* Privilégiez l'utilisation des modificateurs `if/unless` pour les structures sur simple ligne.
  L'utilisation du contrôle de flux `and/or` est une alternative acceptable.

    ```Ruby
    # mauvais
    if some_condition
      do_something
    end

    # bien
    do_something if some_condition

    # alternative acceptable
    some_condition and do_something
    ```

* Privilégiez `unless` plutôt que `if` pour les conditions négatives
  (ou le contrôle de flux  `or`).

    ```Ruby
    # mauvais
    do_something if !some_condition

    # bien
    do_something unless some_condition

    # alternative acceptable
    some_condition or do_something
    ```

* N'utilisez jamais `unless` avec `else`. Réécrivez ces structures
  avec le cas positif en premier.

    ```Ruby
    # mauvais
    unless success?
      puts 'failure'
    else
      puts 'success'
    end

    # bien
    if success?
      puts 'success'
    else
      puts 'failure'
    end
    ```

* N'utilisez pas de parenthèses autour d'une condition `if/unless/while`,
  sauf si la condition contient une affectation (voir "Utilisation de la
  valeur de retour de `=`" ci-dessous).

    ```Ruby
    # mauvais
    if (x > 10)
      # contenu omis
    end

    # bien
    if x > 10
      # contenu omis
    end

    # acceptable
    if (x = self.next_value)
      # contenu omis
    end
    ```

* Privilégiez l'utilisation d'un modificateur `while/until` pour les structures
  à simple ligne.

    ```Ruby
    # mauvais
    while some_condition
      do_something
    end

    # bien
    do_something while some_condition
    ```

* Privilégiez `until` plutôt que `while` pour les conditions négatives.

    ```Ruby
    # mauvais
    do_something while !some_condition

    # bien
    do_something until some_condition
    ```

* Omettez les parenthèses pour les méthodes qui font partie d'un DSL
  (Domain Specific Langage - ex: Rake, Rails, RSpec), qui ont un statut
  de "mot clé" en Ruby (ex: `attr_reader`, `puts`) et les méthodes d'accès
  à des attributs. Utilisez les parenthèses autour des arguments de toutes
  les autres invocations de méthodes.

    ```Ruby
    class Person
      attr_reader :name, :age

      # omis
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

* Privilégiez `{...}` plutôt que `do...end` pour les blocs sur simple ligne.
  Evitez l'utilisation de `{...}` pour les blocs multi-lignes (les enchainements
  multi-lignes sont toujours moches). Utilisez toujours `do...end` pour le
  "flux de contrôle" et les "définitions de méthodes" (ex: dans les Rakefiles
  et certains DSLs). Evitez les `do...end` dans les enchainements.

    ```Ruby
    names = ['Bozhidar', 'Steve', 'Sarah']

    # bien
    names.each { |name| puts name }

    # mauvais
    names.each do |name|
      puts name
    end

    # bien
    names.select { |name| name.start_with?('S') }.map { |name| name.upcase }

    # mauvais
    names.select do |name|
      name.start_with?('S')
    end.map { |name| name.upcase }
    ```

    Certains prétendront que les enchainements multi-lignes ont une bonne apparence en utilisant {...},
    mais ils devraient se demander - est-ce que ce code est vraiment lisible et est-ce que le contenu
    des blocs peut être extrait dans des méthodes efficaces?

* Evitez `return` quand il n'est pas nécessaire pour le flux de contrôle.

    ```Ruby
    # mauvais
    def some_method(some_arr)
      return some_arr.size
    end

    # bien
    def some_method(some_arr)
      some_arr.size
    end
    ```

* Evitez `self` quand il n'est pas nécessaire. (C'est nécessaire uniquement pour appeller un accesseur d'écriture local.)

    ```Ruby
    # mauvais
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # bien
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```

* En conséquence, évitez les ambiguïtés avec des variables locales, sauf si elles sont équivalentes.

    ```Ruby
    class Foo
      attr_accessor :options

      # acceptable
      def initialize(options)
        self.options = options
        # options et self.options sont ici équivalentes
      end

      # mauvais
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # bien
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end
    ```

* Utilisez des espaces autour de l'opérateur `=` lors de l'assignation de valeurs par
 défaut à des paramètres de méthode:

    ```Ruby
    # mauvais
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # bien
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```
    
    Bien que beaucoup de livres Ruby conseillent le premier style, le second est 
    bien plus fréquent (et probablement un peu plus lisible).

* Evitez les prolongements de lignes (\\) quand ils ne sont pas indispensables.
  En pratique, évitez les prolongements de ligne tout court.

    ```Ruby
    # mauvais
    result = 1 - \
             2

    # bien (mais toujours extrêmement moche)
    result = 1 \
             - 2
    ```

* Utiliser la valeur de retour d'un `=` (une affectation) est acceptable,
  mais entourez l'assignement de parenthèses.

    ```Ruby
    # bien - montre l'utilisation prévue de l'affectation
    if (v = array.grep(/foo/)) ...

    # mauvais
    if v = array.grep(/foo/) ...

    # bien également - montre l'utilisation prévue de l'affectation et la priorité est correcte.
    if (v = self.next_value) == 'hello' ...
    ```

* Utilisez `||=` à vonlonté pour initialiser les variables.

    ```Ruby
    # définit Bozhidar comme nom, seulement s'il vaut nil ou false
    name ||= 'Bozhidar'
    ```

* N'utilisez pas `||=` pour initialiser des variables booléennes. (Imaginez ce
  qu'il se passerait si la valeur actuelle était `false`.)

    ```Ruby
    # mauvais - définirait enabled à true, même si elle valait false
    enabled ||= true

    # bien
    enabled = true if enabled.nil?
    ```

* Evitez d'utiliser des variables spéciales de type Perl (comme
  `$0-9`, `$``, etc. ). Elles sont incompréhensibles et toute utilisation dans un
  script de plus d'une ligne est vivement déconseillée.

* Ne mettez jamais d'espace entre le nom d'une méthode et la parenthèse d'ouverture.

    ```Ruby
    # mauvais
    f (3 + 2) + 1

    # bien
    f(3 + 2) + 1
    ```

* Si le premier argument d'une méthode commence par une parenthèse ouvrante,
  utilisez toujours des parenthèses pour l'invocation de méthode. Ecrivez
  par exemple `f((3 + 2) + 1)`.

* Lancez toujours l'interpréteur ruby avec l'option `-w` pour qu'il vous avertisse
  si vous avez oublié l'une des règles ci-dessus!

* La nouvelle notation hash est à privilégier en Ruby 1.9 quand les clés de hash sont des symboles.

    ```Ruby
    # mauvais
    hash = { :one => 1, :two => 2 }

    # bien
    hash = { one: 1, two: 2 }
    ```

* La nouvelle syntaxe lambda est à privilégier en Ruby 1.9.

    ```Ruby
    # mauais
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # bien
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```

* Utilisez `_` pour les paramètres de bloc inutilisés.

    ```Ruby
    # mauvais
    result = hash.map { |k, v| v + 1 }

    # bien
    result = hash.map { |_, v| v + 1 }
    ```

## Nommage

> Les seules vraies difficultés en programmation sont l'invalidation de cache
> et nommer les choses. <br/>
> -- Phil Karlton

* Utilisez le `snake_case` pour les méthodes et les variables.
* Utilisez le `CamelCase` pour les classes et les modules. (Gardez les acronymes comme HTTP,
  RFC, XML en majuscules.)
* Utilisez le `SCREAMING_SNAKE_CASE` pour les autres constantes.
* Le nom des méthodes qui retournent obligatoirement une valeur booléenne doit
  finir par un point d'interrogation.
  (ex: `Array#empty?`).
* Le nom des méthodes potentiellement "dangereuses" (ex: les méthodes qui modifient `self` ou
  les arguments, `exit!` (qui n'exécute pas les finaliseurs contrairement à `exit`), etc.)
  devraient finir par un point d'exclamation (bang) s'il existe une version sécurisée de cette méthode
  *dangereuse*.

    ```Ruby
    # mauvais - il n'y a pas de méthode 'sécurisée'
    class Person
      def update!
      end
    end

    # bien
    class Person
      def update
      end
    end

    # bien
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Définissez la méthode "non bang" (sécurisée) à la suite de la version "bang" (dangereuse)
  si possible.

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* Lorsque vous utilisez `reduce` avec un bloc court, nommez les arguments `|a, e|`
  (accumulator, element).
* Lorsque vous définissez des opérateurs binaires, nommez l'argument `other`.

    ```Ruby
    def +(other)
      # contenu omis
    end
    ```

* Privilégiez `map` plutôt que `collect`, `find` plutôt que `detect`,
  `select` plutôt que `find_all`, `reduce` plutôt que `inject` et
  `size` plutôt que `length`. Ce n'est pas une exigence absolue;
  si l'utilisation de l'alias améliore la lisibilité, il est acceptable
  de l'utiliser. Les methodes qui riment sont un héritage de Smalltalk
  et ne sont pas courantes dans les autres langages de programmation.
  La raison pour laquelle l'utilisation de `select` est encouragée plutôt
  que `find_all` est qu'elle fonctionne bien avec `reject` et que son nom est
  relativement explicite.
  
* Utilisez `flat_map` plutôt que `map` + `flatten`.

    ```Ruby
    # mauvais
    all_songs = users.map(&:songs).flatten.uniq

    # bien
    all_songs = users.flat_map(&:songs).uniq
    ```

## Commentaires

> Un code de qualité constitue sa propre documentation. Quand vous êtes
> sur le point d'ajouter un commentaire, demandez-vous, "Comment puis-je
> améliorer le code pour que ce commentaire ne soit pas nécessaire?"
> Améliorez le code et documentez le ensuite pour le rendre encore plus limpide. <br/>
> -- Steve McConnell

* Ecrivez du code auto-documenté et ignorez le reste de cette section. Vraiment!
* Les commentaires doivent toujours être écrits en anglais pour éviter les problèmes
  liés aux caractères spéciaux.
* La capitalisation et la ponctuation doivent être appliquées aux commentaires
  de plus d'un mot. Utilisez [un espace](http://monsu.desiderio.free.fr/atelier/espace.html) après les virgules.
* Evitez les comemntaires superflus.

    ```Ruby
    # mauvais
    counter += 1 # incrémente le compteur de 1
    ```

* Tenez à jour les commentaires existants. Un commentaire obsolète est pire que pas
  de commentaire du tout.

> Un bon code est comme une bonne blague - il n'a pas besoin d'explication. <br/>
> -- Russ Olsen

* Evitez d'écrire des commentaires pour expliquer un mauvais code.
  Refactorisez ce code pour le rendre explicite.(Fais-le, ou ne le fais pas, mais il n'y a pas d'essai. --Yoda)

### Commentaires d'annotation

* Les annotations devraient généralement être écrites à la ligne juste
  au dessus du code concerné.
* Les mots-clés d'annotation sont suivis par deux points et un espace,
  suivis d'une note décrivant le problème.
* Si plusieurs lignes sont nécessaires pour décrire le problème, les
  lignes suivantes doivent être indentées de deux espaces après  le `#`.

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #   be related to the BarBazUtil upgrade.
      baz(:quux)
    end
    ```

* Si le problème est tellement évident que toute documentation serait
  redondante, les annotations peuvent être laissées à la fin de la ligne
  concernée sans description. Cet usage doit être l'exception et non la règle.

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* Utilisez `TODO` pour marquer les fonctionnalités manquantes ou qui devraient
  être ajoutées ultérieurement.
* Utilisez `FIXME` pour signaler un code erroné qui doit être corrigé.
* Utilisez `OPTIMIZE` pour signaler un code lent ou inefficace pouvant poser
  des problèmes de performance.
* Utilisez `HACK` pour signaler un code qui semble être issu de pratiques
  d'écriture douteuses qui devrait être refactorisé.
* Utilisez `REVIEW` pout signaler toute chose devant être vérifiée pour confirmer
  qu'elle fonctionne comme prévu. Par exemple: `REVIEW: Are we sure this is how the
  client does X currently?`
* Utilisez d'autres mots clés d'annotation personnalisés si vous considérez que c'est
  approprié, mais assurez-vous de les documenter dans le `README` de votre projet.

## Classes

* Quand vous concevez des hiérarchies de classes, assurez-vous qu'elles sont conformes au
  [Principe de substitution de Liskov](http://fr.wikipedia.org/wiki/Principe_de_substitution_de_Liskov).
* Essayez de rendre vos classes aussi
  [SOLIDES](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))
  que possible.
* Fournissez toujours une méthode `to_s` appropriée aux classes qui
  représentent des objets de domaine.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* Utilisez la famille de fonctions `attr` pour définir des accesseurs
  de lecture ou d'écriture.

    ```Ruby
    # mauvais
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # bien
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* Pensez à utiliser `Struct.new`, qui définit les accesseurs triviaux,
  constructeurs et operateurs de comparaison pour vous.

    ```Ruby
    # bien
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # meilleur
    class Person < Struct.new(:first_name, :last_name)
    end
    ````

* Pensez à ajouter des méthodes de fabrique pour faciliter la création
  d'instances d'une classe donnée.

    ```Ruby
    class Person
      def self.create(options_hash)
        # contenu omis
      end
    end
    ```

* Privilégiez le [duck-typing](http://fr.wikipedia.org/wiki/Duck_typing) plutôt que l'héritage.

    ```Ruby
    # mauvais
    class Animal
      # méthode abstraite
      def speak
      end
    end

    # extension de la superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extension de la superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # bien
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* Evitez l'utilisation des variables de classe (`@@`) à cause de leur comportement
  pénible avec l'héritage.

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => va écrire "child"
    ```

    Comme vous pouvez le voir, toutes les classes de la hiérarchie partagent
    en fait la même variable de classe. Les variables d'instance de classe
    devraient généralement être privilégiées à la place des variables de classe.

* Affectez aux méthodes les niveaux de visibilité (`private`, `protected`) appropriés
  conformément à leur utilisation attendue. Ne laissez pas tout en `public`
  (par défaut). Après tout, nous codons en *Ruby* là, pas en *Python*.
* Indentez les méthodes `public`, `protected`, et `private` au même niveau que les
  définitions de methodes auxquelles elles s'appliquent. Passez une ligne au dessus
  du modificateur de visibilité et une ligne en dessous de façon à accentuer le fait
  qu'il s'applique à toutes les méthodes qui le suivent.

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private

      def private_method
        # ...
      end

      def another_private_method
        # ...
      end
    end
    ```

* Utilisez `def self.method` pour définir les méthodes singleton. Cela rend le code plus
  facile à refactoriser car le nom de la classe n'est pas répété.

    ```Ruby
    class TestClass
      # mauvais
      def TestClass.some_method
        # contenu omis
      end

      # bien
      def self.some_other_method
        # contenu omis
      end

      # Egalement possible et pratique quand vous
      # devez définir plusieurs méthodes singleton.
      class << self
        def first_method
          # contenu omis
        end

        def second_method_etc
          # contenu omis
        end
      end
    end
    ```

## Exceptions

* Signalez les exceptions en utilisant la méthode `fail`. Utilisez
  `raise` uniquement lorsque vous capturez une exception et la levez
  à nouveau (parcequ'il ne s'agit pas d'un échec mais d'une levée
  d'exception explicite et intentionnelle).

    ```Ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```

* Ne jamais utiliser `return` dans un bloc `ensure`. Cela donnerait
  la priorité au retour sur une éventuelle levée d'exception,
  et la méthode retournerait un résultat comme si aucune exception
  n'avait été levée. En fait, l'exception serait ignorée silencieusement.

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Utilisez des blocs `begin` implicites autant que possible.

    ```Ruby
    # mauvais
    def foo
      begin
        # algorithme principal
      rescue
        # gestion d'échec
      end
    end

    # bien
    def foo
      # algorithme principal
    rescue
      # gestion d'échec
    end
    ```

* Limitez la prolifération des blocs `begin` en utilisant
  les *méthodes de contingence* (un terme inventé par Avdi Grimm.)

    ```Ruby
    # mauvais
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # bien
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* Ne supprimez pas les exceptions.

    ```Ruby
    # mauvais
    begin
      # une exception se produit ici
    rescue SomeError
      # la section de secours ne fait absolument rien
    end

    # mauvais
    do_something rescue nil
    ```

* Evitez l'utilisation de `rescue` sous sa forme de modificateur.

    ```Ruby
    # mauvais - cela capture toutes les exceptions StandardError
    do_something rescue nil
    ```

* N'utilisez pas d'exception pour le flux de contrôle.

    ```Ruby
    # mauvais
    begin
      n / d
    rescue ZeroDivisionError
      puts 'Cannot divide by 0!'
    end

    # bien
    if d.zero?
      puts 'Cannot divide by 0!'
    else
      n / d
    end
    ```

* Evitez `rescue Exception`. Cela aurait pour effet d'intercepter les
  signaux et appels à `exit`, et nécesiterait de `kill -9` le processus.

    ```Ruby
    # mauvais
    begin
      # les appels de signaux exit et kill sont interceptés (sauf kill -9).
      exit
    rescue Exception
      puts "vous ne vouliez pas vraiment arrêter, pas vrai?"
      # gestion d'exception
    end

    # bien
    begin
      # un rescue par défaut intercepte les exceptions StandardError et non Exception,
      # contrairement à ce que pensent beaucoup de développeurs.
    rescue => e
      # gestion d'exception
    end

    # bien aussi
    begin
      # une exception se produit ici

    rescue StandardError => e
      # gestion d'exception
    end

    ```

* Placez les exceptions spécifiques en haut de la chaine d'interception,
  sans quoi elle ne seront jamais interceptées.

    ```Ruby
    # mauvais
    begin
      # du code
    rescue Exception => e
      # gestion d'exception
    rescue StandardError => e
      # gestion d'exception
    end

    # bien
    begin
      # du code
    rescue StandardError => e
      # gestion d'exception
    rescue Exception => e
      # gestion d'exception
    end
    ```

* Libérez les ressources externes ouvertes par votre programme
  dans un bloc `ensure`.

    ```Ruby
    f = File.open('testfile')
    begin
      # .. processus
    rescue
      # .. gestion d'erreur
    ensure
      f.close unless f.nil?
    end
    ```

* Privilégiez l'utilisation d'exceptions standards plutôt que
  d'introduire de nouvelles classes d'exception.

## Collections

* Privilégiez la notation littérale pour créer des tableaux ou des hashs.
  (sauf si vous devez passer des paramètres à leurs constructeurs).

    ```Ruby
    # mauvais
    arr = Array.new
    hash = Hash.new

    # bien
    arr = []
    hash = {}
    ```

* Privilégiez `%w` à la syntaxe littérale de création de tableau
  quand vous avez besoin d'un tableau de chaines de caractères.

    ```Ruby
    # mauvais
    STATES = ['draft', 'open', 'closed']

    # bien
    STATES = %w(draft open closed)
    ```

* Evitez de créer des écarts énormes dans les tableaux.

    ```Ruby
    arr = []
    arr[100] = 1 # là vous avez un tableau avec plein de nil.
    ```

* Utilisez `Set` plutôt que `Array` pour traiter des éléments uniques.
  `Set` gère une liste de valeurs non ordonnées sans doublons.
  C'est un croisement entre les capacités interopérables et intuitives
  de `Array` et la recherche rapide de `Hash`.
* Privilégiez les symboles plutôt que les chaines de caractères pour
  les clés de hash.

    ```Ruby
    # mauvais
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # bien
    hash = { one: 1, two: 2, three: 3 }
    ```

* Evitez d'utiliser des objets modifiables comme clés de hash.
* Privilégiez la nouvelle notation hash en Ruby 1.9 lorsque vos clés sont des symboles.

    ```Ruby
    # mauvais
    hash = { :one => 1, :two => 2, :three => 3 }

    # bien
    hash = { one: 1, two: 2, three: 3 }
    ```

* Utilisez `fetch` pour manipuler des clés de hash qui sont
  censées être présentes.

    ```Ruby
    heroes = { batman: 'Bruce Wayne', superman: 'Clark Kent' }
    # mauvais - une erreur pourrait ne pas être remarquée
    heroes[:batman] # => "Bruce Wayne"
    heroes[:supermann] # => nil

    # bien - fetch génère une exception KeyError, rendant le problème évident
    heroes.fetch(:supermann)
    ```

* Basez-vous sur le fait que les hash en Ruby 1.9 sont ordonnés.
* Ne jamais modifier une collection en la parcourant.

## Chaînes de caractères

* Privilégiez l'interpolation de chaîne plutôt que la concaténation:

    ```Ruby
    # mauvais
    email_with_name = user.name + ' <' + user.email + '>'

    # bien
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Pensez à entourer le code interpolé d'un espace. Cela permet de mieux
  distinguer le code de la chaîne de caractères.

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* Privilégiez les chaîne à guillemets simples lorsque vous n'utilisez pas
  d'interpolation ou de caractères spéciaux comme `\t`, `\n`, `'`, etc.

    ```Ruby
    # mauvais
    name = "Bozhidar"

    # bien
    name = 'Bozhidar'
    ```

* N'utilisez pas `{}` autour des variables d'instance interpolées dans
  une chaîne.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # mauvais
      def to_s
        "#{@first_name} #{@last_name}"
      end

      # bien
      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* Evitez d'utiliser `String#+` pour assembler de longues chaînes de caractères.
  Utilisez plutôt `String#<<` qui transforme la chaîne sur place et s'avère
  toujours plus rapide que `String#+`, qui crée beaucoup de nouveaux objets.

    ```Ruby
    # bien et rapide
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## Expressions rationnelles

> Certaines personnes se disent, lorsqu'elles sont confrontées à un problème
> "Je sais, je vais utiliser les expressions rationnelles."
> Et elles se retrouvent avec deux problèmes. <br/>
> -- Jamie Zawinski

* N'utilisez pas d'expression rationnelle pour une simple recherche de texte
  en clair dans une chaîne de caractères: `string['text']`
* Pour des situations simples, vous pouvez utiliser la regexp directement
  dans l'index de chaîne.

    ```Ruby
    match = string[/regexp/]             # récupère le contenu correspondant à la regexp
    first_group = string[/text(grp)/, 1] # récupère le contenu du groupe capturé
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    ```

* Utiliser les groupes non capturants quand vous n'utilisez pas le résultat
  capturé par les parenthèses.

    ```Ruby
    /(first|second)/   # mauvais
    /(?:first|second)/ # bien
    ```

* Evitez d'utiliser $1-9 car il peut être difficile de déterminer ce qu'ils
  contiennent. Les groupes nommés peuvent être utilisés à la place.

    ```Ruby
    # mauvais
    /(regexp)/ =~ string
    ...
    process $1

    # bien
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* Les classes de caractères comportent juste quelques caractères spéciaux
  dont vous devez vous soucier: `^`, `-`, `\`, `]`, alors n'échappez pas
  `.` ou les crochets dans `[]`.

* Faites attention avec `^` et `$` car ils correspondent aux débuts et
  fins de lignes, pas de la chaîne.
  Si vous cherchez une correspondance avec la chaîne entière, utilisez
  `\A` et `\z` (à ne pas confondre avec `\Z` qui est l'équivalent de `/\n?\z/`).

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # correspondance
    string[/\Ausername\z/] # pas de correspondance
    ```

* Utilisez le modificateur `x` pour les regexps complexes. Cela les rend plus
  lisibles et vous pouvez ajouter des commentaires utiles. Faites juste
  attention aux espaces qui sont ignorés.

    ```Ruby
    regexp = %r{
      start         # du texte
      \s            # un caractère d'espacement
      (group)       # premier groupe
      (?:alt1|alt2) # une alternative
      end
    }x
    ```

* Pour des remplacements complexes, `sub`/`gsub` peuvent être utilisés
  avec un bloc ou un hash.

## Notations pourcents

* Utilisez `%w` à vonlonté.

    ```Ruby
    STATES = %w(draft open closed)
    ```

* Utilisez `%()` pour les chaînes à simple ligne qui nécessitent à la fois
  une interpolation et l'inclusion de doubles guillemets. Pour les chaînes
  multi-lignes, privilégiez les heredocs.

    ```Ruby
    # mauvais (pas besoin d'interpolation)
    %(<div class="text">Some text</div>)
    # devrait être '<div class="text">Some text</div>'

    # mauvais (pas de double guillemet)
    %(This is #{quality} style)
    # should be "This is #{quality} style"

    # mauvais (multi-lignes)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # bien (interpolation, doubles guillemets, simple ligne)
    %(<tr><td class="name">#{name}</td>)
    ```

* Utilisez `%r` uniquement pour les expressions régulières recherchant *plus d'un* caractère '/'.

    ```Ruby
    # mauvais
    %r(\s+)

    # encore mauvais
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # bien
    %r(^/blog/2011/(.*)$)
    ```

* Evitez `%q`, `%Q`, `%x`, `%s`, and `%W`.

* Privilégiez `()` comme délimiteurs pour toutes les notations `%`.

## Méta-programmation

* Evitez la méta-programmation inutile.

* Ne modifiez pas le comportement des classes du noyau lorsque vous écrivez des librairies.
  (Ne leur appliquez pas de "monkey-patch")

* La forme de bloc de `class_eval` est préférable à la form de chaîne interpolée.
  - Quand vous utilisez une chaîne interpolée, fournissez toujours `__FILE__` et `__LINE__`,
    de sorte que vos piles d'exécution aient du sens:

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` est préférable à `class_eval{ def ... }`

* Lorsque vous utilisez `class_eval` (ou tout autre `eval`) avec une chaîne interpolée,
  ajoutez un bloc de commentaire montrant son apparence une fois interpolée (c'est une
  pratique que j'ai apprise dans le code source de Rails):

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* Evitez d'utilisez `method_missing` pour la méta-programmation. Cela complexifie les piles
  d'exécution; le comportement n'est pas listé dans `#methods`; les appels de méthodes mal
  orthographiés pourraient fonctionner sans générer d'erreur (`nukes.launch_state = false`).
  Pensez à utiliser la délégation, la procuration ou `define_method` à la place. Si vous devez
  vraiment utiliser `method_missing`:
  - [Définissez également `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - Interceptez seulement les méthodes comportant un préfixe bien défini, comme `find_by_*` -- rendez votre code aussi robuste que possible.
  - Appelez `super` à la fin de votre traitement.
  - Déléguez vers des méthodes robustes et non-magiques:

    ```ruby
    # mauvais
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... beaucoup de code pour faire un find_by
      else
        super
      end
    end

    # bien
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # La meilleure solution serait cependant d'utiliser define_method pour chaque attribut
    # utilisable au travers de la méthode find_by.
    ```

## Divers

* Ecrivez du code ruby sécurisé grace à `ruby -w`.
* Evitez les hashs comme paramètres optionnels. La méthode n'en ferait-elle pas trop ?
* Evitez les méthodes de plus de 10 lignes de code (sans compter les lignes vides).
  Idéalement, la plupart des méthodes devraient faire moins de 5 lignes.
* Evitez les listes de paramètres plus longues que trois ou quatre paramètres.
* Si vous avez vraiment besoin de méthodes "globales", ajoutez les au noyau
  en tant que méthodes privées.
* Utilisez les variables d'instances de classe plutôt que des variables globales

    ```Ruby
    # mauvais
    $foo_bar = 1

    # bien
    class Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```
* Evitez d'utiliser `alias` quand vous pouvez utiliser `alias_method`.
* Utilisez `OptionParser` pour analyser des options de ligne de commande
  complexes et `ruby -s` pour des options de ligne de commande simples.
* Codez de manière fonctionnelle, en évitant la mutation autant que possible.
* Ne modifiez pas les arguments sauf si c'est l'objectif de la méthode.
* Evitez les imbrications de blocs sur plus de trois niveaux.
* Soyez cohérent. Dans un monde idéal, soyez cohérent avec ces lignes directrices.
* Utilisez votre bon sens.

# Contribuer

Rien de ce qui est écrit dans ce guide n'est gravé dans le marbre. Mon
but est que nous travaillions ensemble avec toutes les personnes intéressées
par le style de programmation Ruby, de telle sorte que nous puissions créer
une ressource qui serait bénéfique à l'ensemble de la communauté Ruby.

N'hésitez pas à ouvrir des tickets ou d'envoyer des 'pull-requests' avec vos
améliorations. Merci d'avance pour votre aide!

# Passez le mot

Un guide piloté par la communauté n'apportera pas grand chose à une
communauté qui n'est pas au courant de son existence. Tweetez à propos
du guide, partagez le avec vos amis et collègues. Chaque commentaire,
chaque suggestion ou opinion que nous recevons rendra le guide encore
meilleur. Et ce que nous voulons, c'est avoir le meilleur guide possible,
n'est-ce pas?
