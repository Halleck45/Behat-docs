Introduction rapide à Behat
====================

Entrez dans le monde Behat ! Behat est un outil qui rend le "développement
piloté par le comportement" (Beahvior Driven Development, ou BDD) possible.
Avec le BDD, vous écrivez des spécifications, lisibles par des humains, qui
décrivent le fonctionnement de votre application/ Ces spéficiations peuvent être
exécutées pour en tester l'implémentation dans votre application. Et oui,
c'est aussi cool que ça en a l'air !

par exemple, imaginez que vous souhaitez spécifier une application de listing 
de fichier, à savoir la fameus commande UNIX ``ls``.
Voilà comment ça se passerait :

.. code-block:: gherkin

    Fonctionnalité: ls
      Afin de visualiser l'arborescence d'un dossier
      En tant qu'utilisateur UNIX
      Je dois être capable de lister le contenu du répertoire courant

      Scénario: Lister 2 fichiers dans un dossier
        Etant donné que je suis dans le dossier "test"
        Et que j'ai un fichier nommé "foo"
        Et que j'ai un fichier nommé "bar"
        Quand j'exécute la commande "ls"
        Alors je dois obtenir :
          """
          bar
          foo
          """

Avec Behat, vous allez pouvoir exécuter ces scénarios et vérifier que la
commande ``ls`` a le comportement prévu.

Behat peut être utilisé pour tester n'importe quoi, et même une application web
grâce à la librairie `Mink`.

.. note::

    Behat a été inspiré par le projet Ruby _`Cucumber`_.

Installation
------------

behat est un exécutable que vous pouvez lancer en ligne de commande, afin
d'exécuter une série de tests. Afin de commencer, assurez-vous que vous
disposez au moins de la version 5.3.1 de PHP.

Méthode #1 (Composer)
~~~~~~~~~~~~~~~~~~~~

La méthode la plus simple pour installer Behat est de passer par Composer.

Créee un fichier ``composer.json`` à la racine de votre projet

.. code-block:: js

    {
        "require": {
            "behat/behat": ">=2.2.2"
        },

        "config": {
            "bin-dir": "bin/"
        }
    }

Puis téléchargez ``composer.phar`` and et exécutez la commande ``install`` :

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Vous pourrez désormais exécuter Behat :

.. code-block:: bash

    $ bin/behat

Méthode #2 (PEAR)
~~~~~~~~~~~~~~~~

Vous pouvez également installer Behat avec PEAR :

.. code-block:: bash

    $ pear channel-discover pear.symfony.com
    $ pear channel-discover pear.behat.org
    $ pear install behat/behat

Vous pouvez exécuter Behat simplement en lançant la commande ``behat`` :

.. code-block:: bash

    $ behat

Méthode #3 (PHAR)
~~~~~~~~~~~~~~~~

Une autre solution consiste à utiliser une archive PHAR :

.. code-block:: bash

    $ wget https://github.com/downloads/Behat/Behat/behat.phar

Il suffit ensuite de lancer l'archive PHAR avec la commande ``php`` :

.. code-block:: bash

    $ php behat.phar

Méthode #4 (Git)
~~~~~~~~~~~~~~~

Enfin vous pouvez également cloner le projet avec Git en lançant :

.. code-block:: bash

    $ git clone git://github.com/Behat/Behat.git && cd Behat
    $ git submodule update --init

Puis téléchargez ``composer.phar`` et lancez la commande ``install`` :

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Vous pourrez ensuite exécuter Behat avec :

.. code-block:: bash

    $ bin/behat

Utilisation basique
-----------

Dans cet exemple nous allons rapidement tester le comportement de la commande
UNIX ``ls``. Créez un nouveau dossier et initialisez y Behat :

.. code-block:: bash

    $ mkdir ls_project
    $ cd ls_project
    $ behat --init

La commande ``behat --init`` va créer un dossier ``features/`` avec les
composants de base pour démarrer.

Spécifiez votre fonctionnalité
~~~~~~~~~~~~~~~~~~~

Tout dans Behat démarre avec une *fonctionnalité*. Par exemple, ici la 
fonctionnalité consiste en la commande ``ls`` du système UNIX, à savoir "lister 
des fichiers". Commencez donc par créer le fichier ``features/ls.feature`` :

.. code-block:: gherkin

    # features/ls.feature
    Fonctionnalité: ls
      Afin de voir l'arboresence d'un dossier
      En tant qu'utilisateur UNIX
      Je dois être capable de lister le contenu du répertoire courant

Chaque fonctionnalité démarre de la même façon : une ligne qui nomme la
fonctionnalité, suivie de trois lignes qui en décrivent le bénéfice, le rôle et
la fonctionnalité elle-même.

Même si cette section est nécessaire, elle n'est pas indispensable pour Behat.
Si elle est importante, c'est pour que votre fonctionnalité puiss être comprise
et lisible par les autres lecteurs.

Décrire un scénario
~~~~~~~~~~~~~~~~~

Ensuite, ajoutez le scénario suivant à la fin du fichier
``features/ls.feature`` :

.. code-block:: gherkin

    Scénario: Lister 2 fichiers dans un dossier
        Etant donné que je suis dans le dossier "test"
        Et que j'ai un fichier nommé "foo"
        Et que j'ai un fichier nommé "bar"
        Quand j'exécute la commande "ls"
        Alors je dois obtenir :
          """
          bar
          foo
          """

.. tip::

    la syntaxe spéciale ``"""`` dans les dernières lignes permet de définir des
    étapes sur plusieurs lignes. ne vous préocuppez pas pour le moment.

Chaque fonctionnalité est définie par un ou plusieurs "scenarios", qui
décrivent la manière dont la fonctionnalité doit se comporter dans différentes
conditions. C'est cette partie qui va se transformer en test. Chaque
scénario suit toujours le même format de base :

.. code-block:: gherkin

    Scénario: Some description of the scenario
      Etant donné [un contexte]
      Quand [un événement]
      Alors [un résultat attendu]

Chaque étape d'un scénario - le *contexte*, *l'événement* et le *résultat
attendu* - peut être étendue en ajoutant les mots clefs ``Et`` et ``Mais``:

.. code-block:: gherkin

    Scénario: Some description of the scenario
      Etant donné que [un contexte]
      Et [plus d'informations sur le contexte]
      Quand [un événement]
      Et [un autre événement]
      Alors [résultat attendu]
      Et [un autre résultat attendu]
      Mais [un autre résultat attendu]

Il n'y a pas de différence réelle entre ``Alors``, ``Et`` ou ``Mais``, ou aucun
des mot-clefs qui démarrent chaque ligne. Ces mot-clefs sont sont simplement
disponibles dans vos scénarios pour en faciliter la lecture.

Lancer Behat
~~~~~~~~~~~~~~~

Vous venez de définir une fonctionnaltié et un scénario de cette
fonctionnalité. Vous êtes prêt à voir Behat en action ! Exécutez Behat depuis
le dossier de votre projet:

.. code-block:: bash

    $ behat

Si tout fonctionne correctement, vous devriez voir quelque chose comme :

.. image:: /images/ls_no_defined_steps.png
   :align: center

Writing your Step definitions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Behat automatically finds the ``features/ls.feature`` file and tries to execute
its ``Scenario`` as a test. However, we haven't told Behat what to do with
statements like ``Given I am in a directory "test"``, which causes an error.
Behat works by matching each statement of a ``Scenario`` to a list of regular
expression "steps" that you define. In other words, it's your job to tell
Behat what to do when it sees ``Given I am in a directory "test"``. Fortunately,
Behat helps you out by printing the regular expression that you probably
need in order to create that step definition:

.. code-block:: text

    You can implement step definitions for undefined steps with these snippets:

        /**
         * @Given /^I am in a directory "([^"]*)"$/
         */
        public function iAmInADirectory($argument1)
        {
            throw new PendingException();
        }

Let's use Behat's advice and add the following to the ``features/bootstrap/FeatureContext.php``
file, renaming ``$argument1`` to ``$dir``, simply for clarity:

.. code-block:: php

    # features/bootstrap/FeatureContext.php
    <?php

    use Behat\Behat\Context\BehatContext,
        Behat\Behat\Exception\PendingException;
    use Behat\Gherkin\Node\PyStringNode,
        Behat\Gherkin\Node\TableNode;

    class FeatureContext extends BehatContext
    {
        /**
         * @Given /^I am in a directory "([^"]*)"$/
         */
        public function iAmInADirectory($dir)
        {
            if (!file_exists($dir)) {
                mkdir($dir);
            }
            chdir($dir);
        }
    }

Basically, we've started with the regular expression suggested by Behat, which
makes the value inside the quotations (e.g. "test") available as the ``$dir``
variable. Inside the method, we simple create the directory and move into it.

Repeat this for the other three missing steps so that your ``FeatureContext.php``
file looks like this:

.. code-block:: php

    # features/bootstrap/FeatureContext.php
    <?php

    use Behat\Behat\Context\BehatContext,
        Behat\Behat\Exception\PendingException;
    use Behat\Gherkin\Node\PyStringNode,
        Behat\Gherkin\Node\TableNode;

    class FeatureContext extends BehatContext
    {
        private $output;

        /** @Given /^I am in a directory "([^"]*)"$/ */
        public function iAmInADirectory($dir)
        {
            if (!file_exists($dir)) {
                mkdir($dir);
            }
            chdir($dir);
        }

        /** @Given /^I have a file named "([^"]*)"$/ */
        public function iHaveAFileNamed($file)
        {
            touch($file);
        }

        /** @When /^I run "([^"]*)"$/ */
        public function iRun($command)
        {
            exec($command, $output);
            $this->output = trim(implode("\n", $output));
        }

        /** @Then /^I should get:$/ */
        public function iShouldGet(PyStringNode $string)
        {
            if ((string) $string !== $this->output) {
                throw new Exception(
                    "Actual output is:\n" . $this->output
                );
            }
        }
    }

.. note::

    When you specify multi-line step arguments - like we did using the triple
    quotation syntax (``"""``) in the above scenario, the value passed into
    the step function (e.g. ``$string``) is actually an object, which can
    be converted into a string using ``(string) $string`` or
    ``$string->getRaw()``.

Great! Now that you've defined all of your steps, run Behat again:

.. code-block:: bash

    $ behat

.. image:: /images/ls_passing_one_step.png
   :align: center

Success! Behat executed each of your steps - creating a new directory with
two files and running the ``ls`` command - and compared the result to the
expected result.

Of course, now that you've defined your basic steps, adding more scenarios
is easy. For example, add the following to your ``features/ls.feature`` file
so that you now have two scenarios defined:

.. code-block:: gherkin

    Scenario: List 2 files in a directory with the -a option
      Given I am in a directory "test"
      And I have a file named "foo"
      And I have a file named ".bar"
      When I run "ls -a"
      Then I should get:
        """
        .
        ..
        .bar
        foo
        """

Run Behat again. This time, it'll run two tests, and both will pass.

.. image:: /images/ls_passing_two_steps.png
   :align: center

That's it! Now that you've got a few steps defined, you can probably dream
up lots of different scenarios to write for the ``ls`` command. Of course,
this same basic idea could be used to test web applications, and Behat integrates
beautifully with a library called `Mink`_ to do just that.

Of course, there's still lot's more to learn, including more about the
:doc:`Gherkin syntax </guides/1.gherkin>` (the language used in the ``ls.feature``
file).

Some more Behat Basics
----------------------

When you run ``behat --init``, it sets up a directory that looks like this:

.. code-block:: bash

    |-- features
       `-- bootstrap
           `-- FeatureContext.php

Everything related to Behat will live inside the ``features`` directory, which
is composed of three basic areas:

1. ``features/`` - Behat looks for ``*.feature`` files here to execute

2. ``features/bootstrap/`` - Every ``*.php`` file in that directory will
   be autoloaded by Behat before any actual steps are executed

3. ``features/bootstrap/FeatureContext.php`` - This file is the context
   class in which every scenario step will be executed

More about Features
-------------------

As you've already seen, a feature is a simple, readable plain text file,
in a format called Gherkin. Each feature file follows a few basic rules:

1. Every ``*.feature`` file conventionally consists of a single "feature"
   (like the ``ls`` command or *user registration*).

2. A line starting with the keyword ``Feature:`` followed by its title and
   three indented lines defines the start of a new feature.

3. A feature usually contains a list of scenarios. You can write whatever
   you want up until the first scenario: this text will become the feature
   description.

4. Each scenario starts with the ``Scenario:`` keyword followed by a short
   description of the scenario. Under each scenario is a list of steps, which
   must start with one of the following keywords: ``Given``, ``When``, ``Then``,
   ``But`` or ``And``. Behat treats each of these keywords the same, but you
   should use them as intended for consistent scenarios.

.. tip::

    Behat also allows you to write your features in your native language.
    In other words, instead of writing ``Feature``, ``Scenario`` or ``Given``,
    you can use your native language by configuring Behat to use one of its
    many supported languages.
    
    To check if your language is supported and to see the available keywords,
    run:
    
    .. code-block:: bash
    
        $ behat --story-syntax --lang YOUR_LANG

    Supported languages include (but are not limited to) ``fr``, ``es``, ``it``
    and, of course, the english pirate dialect ``en-pirate``.

    Keep in mind, that any language, different from ``en`` should be explicitly
    marked with ``# language: ...`` comment at the begining of your
    ``*.feature`` file:

    .. code-block:: gherkin

        # language: fr
        Fonctionnalité: ...
          ...

You can read more about features and Gherkin language in ":doc:`/guides/1.gherkin`"
guide.

More about Steps
----------------

For each step (e.g. ``Given I am in a directory "test"``), Behat will look
for a matching step definition by matching the text of the step against the
regular expressions defined by each step definition.

A step definition is written in php and consists of a keyword, a regular
expression, and a callback. For example:

.. code-block:: php

    /**
     * @Given /^I am in a directory "([^"]*)"$/
     */
    public function iAmInADirectory($dir)
    {
        if (!file_exists($dir)) {
            mkdir($dir);
        }
        chdir($dir);
    }

A few pointers:

1. ``@Given`` is a definition keyword. There are 3 supported keywords in
   annotations: ``@Given``/``@When``/``@Then``. These three definition keywords
   are actually equivalent, but all three are available so that your step
   definition remains readable.

2. The text after the keyword is the regular expression (e.g. ``/^I am in a directory "([^"]*)"$/``).

3. All search patterns in the regular expression (e.g. ``([^"]*)``) will become
   method arguments (``$dir``).

4. If, inside a step, you need to tell Behat that some sort of "failure" has
   occurred, you should throw an exception:

    .. code-block:: php

       /**
        * @Then /^I should get:$/
        */
       public function iShouldGet(PyStringNode $string)
       {
           if ((string) $string !== $this->output) {
               throw new Exception(
                   "Actual output is:\n" . $this->output
               );
           }
       }

.. tip::

    Behat doesn't come with its own assertion tool, but you can use any proper
    assertion tool out there. Proper assertion tool is a library, which
    assertions throw exceptions on fail. For example, if you're familiar with
    PHPUnit, you can use its assertions in Behat:

    .. code-block:: php

        # features/bootstrap/FeatureContext.php
        <?php

        use Behat\Behat\Context\BehatContext;
        use Behat\Gherkin\Node\PyStringNode;

        require_once 'PHPUnit/Autoload.php';
        require_once 'PHPUnit/Framework/Assert/Functions.php';

        class FeatureContext extends BehatContext
        {
            /**
             * @Then /^I should get:$/
             */
            public function iShouldGet(PyStringNode $string)
            {
                assertEquals($string->getRaw(), $this->output);
            }
        }

In the same way, any step that does *not* throw an exception will be seen
by Behat as "passing". 

You can read more about step definitions in ":doc:`/guides/2.definitions`" guide.

The Context Class: ``FeatureContext``
-------------------------------------

Behat creates a context object for each scenario and executes all scenario
steps inside that same object. In other words, if you want to share variables
between steps, you can easily do that by setting property values on the context
object itself (which was shown in the previous example).

You can read more about ``FeatureContext`` in ":doc:`/guides/4.context`" guide.

The ``behat`` Command Line Tool
-------------------------------

Behat comes with a powerful console utility responsible for executing the
Behat tests. The utility comes with a wide array of options.

To see options and usage for the utility, run:

.. code-block:: bash

    $ behat -h

One of the handiest things it does it to show you all of the step definitions
that you have configured in your system. This is an easy way to recall exactly
how a step you defined earlier is worded:

.. code-block:: bash

    $ behat -dl

You can read more about Behat CLI in ":doc:`/guides/6.cli`" guide.

What's Next?
------------

Congratulations! You now know everything you need in order to get started
with behavior driven development and Behat. From here, you can learn more
about the :doc:`Gherkin</guides/1.gherkin>` syntax or learn how to test your
web applications by using Behat with Mink.

* :doc:`/cookbook/behat_and_mink`
* :doc:`/guides/1.gherkin`
* :doc:`/guides/6.cli`

.. _`behavior driven development`: http://en.wikipedia.org/wiki/Behavior_Driven_Development
.. _`Mink`: https://github.com/behat/mink
.. _`What's in a Story?`: http://blog.dannorth.net/whats-in-a-story/
.. _`Cucumber`: http://cukes.info/
.. _`Goutte`: https://github.com/fabpot/goutte
.. _`PHPUnit`: http://phpunit.de
.. _`Testing Web Applications with Mink`: https://github.com/behat/mink
