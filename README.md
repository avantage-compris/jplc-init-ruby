jplc-init-ruby
==============

### 1. Objectif

Réaliser en Ruby le portage d’un programme simple écrit en FreeBASIC.

### 2. Fichiers de référence

Le programme original « FillCov » 
lit un fichier CSV comportant des données manquantes,
infère des valeurs pour ces données manquantes à partir des données présentes,
et émet deux fichiers CSV en sortie : le fichier complété avec les données
trouvées, et la matrice symétrique de covariance entre les colonnes de
données du fichier complété.

Fichiers CSV de référence :

  * UKOutput.csv : exemple de fichier en entrée
  * OupFillCov.csv : exemple de sortie, premier fichier
  * OupFillFile.csv : exemple de sortie, deuxième fichier

### 3. Utiliser git sur son poste

#### 3.1. Récupérer une première fois le repository git sur son poste

En ligne de commande :

    > git clone https://github.com/avantage-compris/jplc-init-ruby
    > cd jplc-init-ruby
    
#### 3.2. Enregistrer les modifications dans git sur son poste

Pour vérifier qu’il y a des modifications en cours :

    > git status

Pour voir le détail des modifications en cours :

    > git diff
    
Pour enregistrer les modifications dans git :

    > git commit -a -m 'Nouvel essai'

#### 3.3. Voir les dernières modifications enregistrées sur son poste

    > git log    
    commit xxx
    Author: David Andrianavalontsalama <david.andriana@avantage-compris.com>
    Date:   Tue Jul 26 06:50:33 2016 +0200

        Initial import: README + CSV test files + helloworld

#### 3.4. Voir le détail d’une modification (différences avant/après)

    > git show xxx

#### 3.5. Revenir à un état antérieur

    > git checkout xxx

#### 3.6. Revenir à l’état le plus récent

    > git checkout master

### 4. Utiliser Ruby sur son poste

#### 4.1. Ligne de commande interactive

Lancer l’environnement « irb » :

    $ irb
    >> puts "Hello World!"
    Hello World!
    => nil

En Ruby, tout a une valeur de retour, même si c’est « nil » :

    >> 3 + 4
    => 7

Un peu de manipulation de chaînes de caractères :

    >> 'Hello World!'
    => "Hello World!"
    >> a = 'John'
    => "John"
    >> s = "Hello #{a}"
    => "Hello John"
    >> s.length
    => 10
    >> s[5]
    => 32

Tout est objet :

    >> s[6]
    => 74
    >> 74.chr
    => "J"
    >> nil
    => nil
    >> nil.nil?
    => true
    >> nil.to_s
    => ""
    >> nil.to_s.nil?
    => false
    >> nil.to_a
    => []

Méthode :

    >> def add(a,b)
    >>    a + b
    >> end
    => nil
    >> add 4,5
    => 9

 Méthode (où est le bug ?) :

    >> def r2(a,b)
    >>   fail if a.length != b.length
    >>   sq = (0...a.length).map { |i| (a[i] - b[i]) ** 2 }
    >>   sq.inject(0) { |sum, x| sum + x } / a.length
    >> end
    => nil
    >> r2 [4, 5, 6], [7, 8, 9]
    => 9
    >> r2 [4, 5, 6, 8], [6, 7, 8, 9]
    => 3
    >> r2 [4, 5, 6, 8, 9], [5, 6, 7, 8, 9]
    => 0    
    >> r2 [4.0, 5, 6, 8, 9], [5, 6, 7, 8, 9]
    => 0.6    
    >> r2 [4.0, 5, 6, 8], [6, 7, 8, 9]
    => 3.25
    
Identifiant d’un objet (voir aussi la méthode « equal? ») :

    >> "Hello".object_id
    => 2177350020
    >> "Hello".object_id
    => 2177345540
    >> 74.object_id
    => 149
    >> 74.object_id
    => 149
    >> :Hello.object_id
    => 446988
    >> :Hello.object_id
    => 446988

Extraction de sous-chaîne :

    >> s.slice(3, 2)
    => "lo"
    >> s[3..5]
    => "lo"

Méthodes non destructrices / destructrices :

    >> s.slice(3, 2)
    => "lo"
    >> s
    => "Hello John"
    >> s.slice!(3, 2)
    => "lo"
    >> s
    => "Hel John"
    
Une chaîne de caractères se comporte un peu comme un tableau :

    >> s = 'Hel John'
    => "Hel John"
    >> s[2] = 'k'
    => "k"
    >> s
    => "Hek John"
    >> s[2] = 'llo'
    => "llo"
    >> s
    => "Hello John"
    >> s[6..9] = 'Mary'; s
    => "Hello Mary"
    >> s[6...10] = 'Will'; s
    => "Hello Will"
    >> s[5] = ', and good morning '; s
    => "Hello, and good morning Will"
    >> s = 'Hello John'
    => "Hello John"
    >> s[2..1] = '-'; s[s.length..s.length] = '!'; s
    => "He-llo John!"
    >> s << "..."
    => "He-llo John!…"
    
D’une chaîne de caractères à un tableau :

    >> "Hello".to_a
    => ["Hello"]
    >> "Hello".each_char.to_a
    => ["H", "e", "l", "l", "o"]
    >> "Hello".each_char.map do |c| c[0]; end
    => [72, 101, 108, 108, 111]

Égalités en Ruby :

    >> "Hello" == "Hello"
    => true
    >> "Hello" === "hello"
    => true
    >> "Hello".eql? "Hello"
    => true
    >> 'abcd' <=> 'efgh'
    => -1
    >> "Hello".equal? "Hello"
    => false

Égalités sur des nombres :

    >> 1 == 1.0
    => true
    >> 1 === 1.0
    => true
    >> 1.eql? 1.0
    => false
    >> 145 <=> 232
    => -1
    >> 1.equal? 1
    => true

Un peu de manipulation de tableaux :

    >> a = [4, 5, 7, 8, 9, 10]
    => [4, 5, 7, 8, 9, 10]
    >> a.select { |x| x % 2 == 1 }
    => [5, 7, 9]
    >> a
    => [4, 5, 7, 8, 9, 10]
    >> a.delete_if { |x| x % 2 != 1 }
    => [5, 7, 9]
    >> a
    => [5, 7, 9]
    >> a.each do |n| puts n**2; end
    25
    49
    81
    => [5, 7, 9]
    >> a.map do |n| n**2; end
    => [25, 49, 81]
    >> a
    => [5, 7, 9]
    >> a.map! do |n| n**2; end
    => [25, 49, 81]
    >> a
    => [25, 49, 81]
    >> a.inject(0) do |s, x| s += x; end
    => 155
    >> a.each_with_index.map 
    => [[25, 0], [49, 1], [81, 2]]
    >> a.each_with_index.map { |(x, index)| index**2 }
    => [0, 1, 4]
    >> a.each_with_index.inject(0) do |s, (x, index)| s += x * index; end
    => 211
    >> a.map!.with_index.map
    => [[25, 0], [49, 1], [81, 2]]
    >> a
    => [nil, nil, nil]
    >> a = [25, 49, 81]
    => [25, 49, 81]
    >> a.map!.with_index do |c, i| [c, i]; end
    => [[25, 0], [49, 1], [81, 2]]
    >> a
    => [[25, 0], [49, 1], [81, 2]]
    >> a.map! { |x| x.first }
    => [25, 49, 81]

#### 4.2. Fichier sources

Suffixe « .rb ».

En ligne de commande UNIX :

    > ruby helloworld.rb

En ligne de commande DOS :

    > ruby helloworld.rb

Commentaires en Ruby :

    # This is a comment line 
    
    nombre_de_fenetres = 3 # Now, nombre_de_fenetres is 3
    
    =begin
    ...
    =end
    
Pour invoquer depuis un fichier Ruby un autre fichier Ruby :

    include 'mysubfile.rb'
