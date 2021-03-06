.. index::
   single: Forms; Fields; choice

Type de champ Entity
====================

Un champ ``choice`` spécial qui est conçu pour charger ses options d'une entité
Doctrine. Par exemple, si vous avez une entité ``Category``, vous pourrez utiliser
ce champ pour afficher une liste ``select`` de tout ou de certains objets ``Category``
depuis la base de données.

+-------------+----------------------------------------------------------------------+
| Rendu comme | peut être plusieurs balises (voir :ref:`forms-reference-choice-tags`)|
+-------------+----------------------------------------------------------------------+
| Options     | - `class`_                                                           |
|             | - `data_class`_                                                      |
|             | - `property`_                                                        |
|             | - `group_by`_                                                        |
|             | - `query_builder`_                                                   |
|             | - `em`_                                                              |
+-------------+----------------------------------------------------------------------+
| Options     | - `choices`                                                          |
| surchargées | - `choice_list`                                                      |
+-------------+----------------------------------------------------------------------+
| Options     | - `multiple`_                                                        |
| héritées    | - `expanded`_                                                        |
|             | - `preferred_choices`_                                               |
|             | - `empty_value`_                                                     |
|             | - `empty_data`_                                                      |
|             | - `required`_                                                        |
|             | - `label`_                                                           |
|             | - `label_attr`_                                                      |
|             | - `data`_                                                            |
|             | - `read_only`_                                                       |
|             | - `disabled`_                                                        |
|             | - `error_bubbling`_                                                  |
|             | - `error_mapping`_                                                   |
|             | - `mapped`_                                                          |
+-------------+----------------------------------------------------------------------+
| Type parent | :doc:`choice</reference/forms/types/choice>`                         |
+-------------+----------------------------------------------------------------------+
| Classe      | :class:`Symfony\\Bridge\\Doctrine\\Form\\Type\\EntityType`           |
+-------------+----------------------------------------------------------------------+

Utilisation de base
-------------------

Le type ``entity`` n'a qu'une seule option obligatoire : l'entité qui doit être listée
dans le champ Choice::

    $builder->add('users', 'entity', array(
        'class' => 'AcmeHelloBundle:User',
        'property' => 'username',
    ));

Dans ce cas, tout les objets ``User`` seront chargés depuis la base de données et seront
affichés soit comme une balise ``select``, soit un ensemble de boutons radio ou de checkboxes
(cela dépendra des valeurs des options ``multiple`` et ``expanded``). Si l'objet entité ne
possède pas de méthode ``__toString()``, alors l'option ``property`` est nécessaire. 

Utiliser une requête personnalisée pour les entités
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Si vous avez besoin de spécifier une requête personnalisée à utiliser pour charger
les entitées (par exemple si vous ne voulez que certaines entités, ou que vous voulez
les classer), utilisez l'option ``query_builder``. La façon la plus simple d'utiliser
cette option est la suivante::

    use Doctrine\ORM\EntityRepository;
    // ...

    $builder->add('users', 'entity', array(
        'class' => 'AcmeHelloBundle:User',
        'query_builder' => function(EntityRepository $er) {
            return $er->createQueryBuilder('u')
                ->orderBy('u.username', 'ASC');
        },
    ));

.. include:: /reference/forms/types/options/select_how_rendered.rst.inc

Options du champ
----------------

class
~~~~~

**type**: ``string`` **obligatoire**

La classe de votre entité (ex: ``AcmeStoreBundle:Category``). Cela peut être
le nom complet de la classe (ex: ``Acme\StoreBundle\Entity\Category``) ou son alias
(voir ci-dessus).

.. include:: /reference/forms/types/options/data_class.rst.inc

property
~~~~~~~~

**type**: ``string``

C'est la propriété qui doit être utilisée pour afficher l'entité sous forme de
texte dans l'élément HTML. Si vous le laissez vide, l'objet entité sera converti
en texte et devra alors implémenter la méthode ``__toString()``.

group_by
~~~~~~~~

**type**: ``string``

C'est un nom de propriété (ex ``author.name``) utilisé pour organiser
les choix disponibles dans les groupes. Cette propriété ne fonctionne que lorsque vous
affichez une balise select, et cela se fait par l'ajout de balises optgroup
autour des balises option. Les choix qui ne retournent aucune valeur pour ce nom
de propriété sont affichés directement dans la balise select, sans optgroup.

query_builder
~~~~~~~~~~~~~

**type**: ``Doctrine\ORM\QueryBuilder`` or a Closure

Si elle est spécifiée, cette option sera utilisée pour requêter un sous-ensemble
d'objets (et leur classement) qui sera affiché dans le champ. La valeur de cette
option peut être soit un objet ``QueryBuilder`` soit une Closure. Si vous utilisez
une Closure, elle ne doit prendre qu'un seul argument qui est l'objet ``EntityRepository``
de l'entité.

em
~~

**type**: ``string`` **default**: le gestionnaire d'entité par défaut

Si elle est spécifiée, cette option définit le gestionnaire d'entité (entity manager)
qui sera utilisé pour charger les objets au lieu du gestionnaire par défaut.

Options surchargées
-------------------

choices
~~~~~~~

**default**: ``null``

choice_list
~~~~~~~~~~~

**default**: toutes les entités sélectionnées

Le paramétrage par défaut de ``choices`` sélectionne toutes les entités en utilisant
l'une des options documentées ci-dessus.

Options héritées
----------------

Ces options sont héritées du type :doc:`choice</reference/forms/types/choice>` :

.. include:: /reference/forms/types/options/multiple.rst.inc

.. note::

    Si vous utilisez les collections d'entités Doctrine, il vous sera utile
    de lire la documention pour :doc:`/reference/forms/types/collection`
    De plus, il y a un exemple complet dans le cookbook.
    :doc:`/cookbook/form/form_collections`.

.. include:: /reference/forms/types/options/expanded.rst.inc

.. include:: /reference/forms/types/options/preferred_choices.rst.inc

.. note::
    
    Cette option attend un tableau d'objets, contrairement au champ
    ``choice`` qui demande un tableau de clés.

.. include:: /reference/forms/types/options/empty_value.rst.inc

Ces options sont héritées du type :doc:`form</reference/forms/types/form>` :

.. include:: /reference/forms/types/options/empty_data.rst.inc

.. include:: /reference/forms/types/options/required.rst.inc

.. include:: /reference/forms/types/options/label.rst.inc

.. include:: /reference/forms/types/options/label_attr.rst.inc

.. include:: /reference/forms/types/options/data.rst.inc

.. include:: /reference/forms/types/options/read_only.rst.inc

.. include:: /reference/forms/types/options/disabled.rst.inc

.. include:: /reference/forms/types/options/error_bubbling.rst.inc

.. include:: /reference/forms/types/options/error_mapping.rst.inc

.. include:: /reference/forms/types/options/mapped.rst.inc
