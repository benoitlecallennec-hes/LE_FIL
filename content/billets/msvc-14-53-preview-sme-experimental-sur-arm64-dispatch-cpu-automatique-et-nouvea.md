---
title: "MSVC 14.53 Preview : SME expérimental sur ARM64, dispatch CPU automatique et nouveaux contrôles d'extensions"
date: 2026-11-13T18:12:10.000Z
source: Microsoft C++ Team Blog
link: https://devblogs.microsoft.com/cppblog/msvc-build-tools-preview-updates-november-2026/
tags:
  - cpp
feed_tag: MSVC
lang: en
level: advanced
format: article
reading_time: 4
summary: La mise à jour de novembre 2026 des MSVC Build Tools Preview, qui prépare la version 14.53, touche le frontend, la génération de code ARM64 et x64, l'optimiseur, l'éditeur de liens et AddressSanitizer.
takeaways:
  - "[[fallthrough]] accepté en C++14, et nouvelles options /Zc:__super et /Zc:__if_exists pour désactiver ces extensions Microsoft"
  - Prise en charge expérimentale et partielle de SME sur ARM64 natif via /feature:sme
  - "Nouvel attribut [[msvc::auto_dispatch]] : variantes par CPU générées à partir d'un seul corps de fonction, choisies au chargement (LTCG)"
  - Option /WARNUNUSEDLIBS du linker pour repérer les bibliothèques inutilisées
status: rejected
draft: true
featured: false
---
Microsoft présente les changements du dernier mois dans les MSVC Build Tools Preview, qui préparent la version 14.53. On les installe via les canaux Stable ou Insiders de Visual Studio 2026, et Insiders les reçoit plus souvent. cl.exe et link.exe doivent afficher au moins la version 19.53.36930 / 14.53.36930. Côté frontend, l'attribut [[fallthrough]] est maintenant accepté en mode C++14. Deux avertissements de niveau 4, C4878 et C4879, désactivés par défaut, signalent les conversions entre HRESULT ou SCODE et les types de type booléen. Les options /Zc:__super[-] et /Zc:__if_exists[-] activent ou désactivent ces deux extensions Microsoft. Le frontend consomme aussi moins de mémoire grâce à une meilleure gestion des jetons de source mis en tampon. Enfin, -fsanitize-coverage-ignorelist devient un autre nom pour -fsanitize-coverage-blocklist.

Sur ARM64, la prise en charge expérimentale de l'extension SME (Scalable Matrix Extension), activée par /feature:sme, s'étend : déclarations de fonctions « streaming », appels directs et indirects, LTCG, gestion des exceptions et fonctions d'aide pour l'état du processeur. Elle reste partielle, limitée à l'ARM64 natif, et ne gère ni les définitions de fonctions streaming ni ARM64EC. On trouve aussi la constante ARM64_FPMR pour _ReadStatusReg et _WriteStatusReg, un inlining qui tient compte de la pression sur les registres flottants et vectoriels, et des séquences NEON d'extension par zéro plus courtes. Sur x64 et x86, le compilateur génère plus de comparaisons conditionnelles APX, y compris pour les switch, et choisit mieux les déplacements conditionnels. Il produit de nouveau des épilogues compacts pour la version 3 des données de déroulement (unwind), combine certaines séquences AVX-512 en instructions à masque de fusion et remplace certaines multiplications par des constantes par des calculs d'adresse plus courts.

L'optimiseur gagne un attribut opt-in, [[msvc::auto_dispatch]], qui génère avec la LTCG des variantes spécifiques à chaque CPU à partir d'un seul corps de fonction. Un chargeur compatible choisit ensuite la bonne variante au chargement. D'autres gains concernent l'inlining (simplification des agrégats, suppression des écritures inutiles), le remplacement de divisions par une constante par des multiplications et décalages, la suppression de tests redondants dans les boucles et une vectorisation plus large, avec une prise en charge partielle des groupes de trois éléments. S'y ajoutent un meilleur loop unswitching et moins de padding dans les données RTTI et EH, ce qui réduit la taille des binaires.

L'éditeur de liens propose une option opt-in, /WARNUNUSEDLIBS : elle émet l'avertissement LNK4324 pour toute bibliothèque d'entrée dont aucun membre n'est chargé. Les liens incrémentaux sont aussi plus rapides quand les entrées sont nombreuses. MASM accepte plus d'instructions d'épilogue avec la version 3 des données de déroulement. L'analyse statique applique désormais C26433 et C26435 aux destructeurs virtuels redéfinis. AddressSanitizer gagne une option windows_shadow_top_down (Windows 64 bits) qui place la mémoire fantôme en haut de l'espace d'adressage, utile quand une DLL instrumentée est chargée dans une application .NET Framework. Pour la bibliothèque standard, l'article renvoie au changelog STL sur GitHub, et il se termine par une longue liste de tickets Developer Community corrigés.
