Documentazione su API
=====================

Grazie a questa documentazione, vedremo come interagire con l'API di
wallabag.

Requisiti
=========

-   wallabag appena (o no) installato su <http://localhost:8000>
-   `httpie` installato sul vostro computer ([guardate il sito del
    progetto](https://github.com/jkbrzt/httpie)). Da notare che potete
    anche adattare i comandi usando curl o wget.
-   tutti i metodi API documentati qui <http://localhost:8000/api/doc>
    (sulla vostra istanza) e [sulla nostra istanza
    d'esempio](http://v2.wallabag.org/api/doc)

Creare un nuovo client API
==========================

Sul vostro account wallabag potete creare un nuovo client API presso
questo URL <http://localhost:8000/developer/client/create>.

Date solamente l'URL per il reindirizzamento della vostra applicazione e
create il vostro client. Se la vostra applicazione è desktop, inserite
l'URL che preferite.

Toverete informazioni come queste:

    Client ID:

    1_3o53gl30vhgk0c8ks4cocww08o84448osgo40wgw4gwkoo8skc

    Client secret:

    636ocbqo978ckw0gsw4gcwwocg8044sco0w8w84cws48ggogs4

Ottenere un token per il ricaricamento
======================================

Per ogni chiamata API avrete bisogno di un token. Creiamolo con questo
comando (rimpiazzate `client_id`, `client_secret`, `username` and
`password` con i loro valori):

    http POST http://localhost:8000/oauth/v2/token \
        grant_type=password \
        client_id=1_3o53gl30vhgk0c8ks4cocww08o84448osgo40wgw4gwkoo8skc \
        client_secret=636ocbqo978ckw0gsw4gcwwocg8044sco0w8w84cws48ggogs4 \
        username=wallabag \
        password=wallabag

Otterrete questo risultato:

    HTTP/1.1 200 OK
    Cache-Control: no-store, private
    Connection: close
    Content-Type: application/json
    Date: Tue, 05 Apr 2016 08:44:33 GMT
    Host: localhost:8000
    Pragma: no-cache
    X-Debug-Token: 19c8e0
    X-Debug-Token-Link: /_profiler/19c8e0
    X-Powered-By: PHP/7.0.4

    {
        "access_token": "ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA",
        "expires_in": 3600,
        "refresh_token": "OTNlZGE5OTJjNWQwYzc2NDI5ZGE5MDg3ZTNjNmNkYTY0ZWZhZDVhNDBkZTc1ZTNiMmQ0MjQ0OThlNTFjNTQyMQ",
        "scope": null,
        "token_type": "bearer"
    }

Lavoreremo con il valore `access_token` nelle nostre prossime chiamate.

esempio di cURL:

    curl -s "https://localhost:8000/oauth/v2/token?grant_type=password&client_id=1_3o53gl30vhgk0c8ks4cocww08o84448osgo40wgw4gwkoo8skc&client_secret=636ocbqo978ckw0gsw4gcwwocg8044sco0w8w84cws48ggogs4&username=wallabag&password=wallabag"

Ottenere voci esistenti
=======================

Documentazione per questo metodo:
<http://localhost:8000/api/doc#get--api-entries>.{\_format}

Mentre lavoriamo su una nuova installazione di wallabag, non otterremo
risultati con questo comando:

    http GET http://localhost:8000/api/entries.json \
    "Authorization:Bearer ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA"

restituisce:

    HTTP/1.1 200 OK
    0: application/json
    Cache-Control: no-cache
    Connection: close
    Content-Type: application/json
    Date: Tue, 05 Apr 2016 08:51:32 GMT
    Host: localhost:8000
    Set-Cookie: PHPSESSID=nrogm748md610ovhu6j70c3q63; path=/; HttpOnly
    X-Debug-Token: 4fbbc4
    X-Debug-Token-Link: /_profiler/4fbbc4
    X-Powered-By: PHP/7.0.4

    {
        "_embedded": {
            "items": []
        },
        "_links": {
            "first": {
                "href": "http://localhost:8000/api/entries?page=1&perPage=30"
            },
            "last": {
                "href": "http://localhost:8000/api/entries?page=1&perPage=30"
            },
            "self": {
                "href": "http://localhost:8000/api/entries?page=1&perPage=30"
            }
        },
        "limit": 30,
        "page": 1,
        "pages": 1,
        "total": 0
    }

L'insieme `items` è vuoto.

esempio di cURL:

    curl --get "https://localhost:8000/api/entries.html?access_token=ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA"

Aggiungere la vostra prima voce
===============================

Documentazione per questo metodo:
<http://localhost:8000/api/doc#post--api-entries>.{\_format}

    http POST http://localhost:8000/api/entries.json \
    "Authorization:Bearer ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA" \
    url="http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html"

restituisce:

    HTTP/1.1 200 OK
    0: application/json
    Cache-Control: no-cache
    Connection: close
    Content-Type: application/json
    Date: Tue, 05 Apr 2016 09:07:54 GMT
    Host: localhost:8000
    Set-Cookie: PHPSESSID=bjie40ck72kp2pst3i71gf43a4; path=/; HttpOnly
    X-Debug-Token: e01c51
    X-Debug-Token-Link: /_profiler/e01c51
    X-Powered-By: PHP/7.0.4

    {
        "_links": {
            "self": {
                "href": "/api/entries/1"
            }
        },
        "content": "<p class=\"chapo\">Fonctionnant sur le même principe que Pocket, Instapaper ou Readability, le logiciel Wallabag permet de mémoriser des articles pour les lire plus tard. Sa nouvelle version apporte une multitude de nouvelles fonctionnalités.</p><p>Si vous utilisez Firefox comme navigateur web, vous avez peut-être constaté l’arrivée d’<a href=\"http://www.numerama.com/magazine/33292-update-firefox.html\">une fonctionnalité intitulée Pocket</a>. Disponible autrefois sous la forme d’un module complémentaire, et sous un autre nom (Read it Later), elle est depuis le mois de juin 2015 directement incluse au sein de Firefox.</p>\n<p>Concrètement, Pocket sert à garder en mémoire des contenus que vous croisez au fil de la navigation, comme des articles de presse ou des vidéos, afin de pouvoir les consulter plus tard. Pocket fonctionne un peu comme un système de favoris, mais en bien plus élaboré grâce à ses options supplémentaires.</p>\n<p>Mais <a href=\"https://en.wikipedia.org/wiki/Pocket_%28application%29#Firefox_integration\" target=\"_blank\">Pocket fait polémique</a>, car il s’agit d’un projet propriétaire qui est intégré dans un logiciel libre. C’est pour cette raison que des utilisateurs ont choisi de se tourner vers d’autres solutions, comme <strong>Wallabag</strong>, qui est l’équivalent libre de Pocket et d’autres systèmes du même genre, comme Instapaper et Readability.</p>\n<p>Et justement, Wallabag évolue. C’est ce dimanche que la <a href=\"https://www.wallabag.org/blog/2016/04/03/wallabag-v2\" target=\"_blank\">version 2.0.0 du logiciel</a> a été publiée par l’équipe en  charge de son développement et celle-ci contient de nombreux changements par rapport aux moutures précédentes (la <a href=\"http://doc.wallabag.org/fr/v2/\" target=\"_blank\">documentation est traduite</a> en français), lui permettant d’apparaître comme une alternative à Pocket, Instapaper et Readability.</p>\n<p><img class=\"aligncenter size-medium wp-image-160439\" src=\"http://www.numerama.com/content/uploads/2016/04/homepage-680x347.png\" alt=\"homepage\" width=\"680\" height=\"347\" srcset=\"//www.numerama.com/content/uploads/2016/04/homepage-680x347.png 680w, //www.numerama.com/content/uploads/2016/04/homepage-1024x523.png 1024w, //www.numerama.com/content/uploads/2016/04/homepage-270x138.png 270w, //www.numerama.com/content/uploads/2016/04/homepage.png 1286w\" sizes=\"(max-width: 680px) 100vw, 680px\"/></p>\n<p>Parmi les principaux changements que l’on peut retenir avec cette nouvelle version, notons la possibilité d’écrire des annotations dans les articles mémorisés, de filtrer les contenus selon divers critères (temps de lecture, nom de domaine, date de création, statut…), d’assigner des mots-clés aux entrées, de modifier le titre des articles, le support des flux RSS ou encore le support de plusieurs langues dont le français.</p>\n<p>D’autres options sont également à signaler, comme l’aperçu d’un article mémorisé (si l’option est disponible), un guide de démarrage rapide pour les débutants, un outil d’export dans divers formats (PDF, JSON, EPUB, MOBI, XML, CSV et TXT) et, surtout, la possibilité de migrer vers Wallabag depuis Pocket, afin de convaincre les usagers de se lancer.</p>\n    \n    \n    <footer class=\"clearfix\" readability=\"1\"><p class=\"source\">\n        Crédit photo de la une : <a href=\"https://www.flickr.com/photos/bookgrl/2388310523/\">Laura Taylor</a>\n    </p>\n    \n    <p><a href=\"http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html?&amp;show_reader_reports\" target=\"_blank\" rel=\"nofollow\">Signaler une erreur dans le texte</a></p>\n        \n</footer>    <section class=\"related-article\"><header><h3>Articles liés</h3>\n    </header><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/magazine/34444-firefox-prepare-l-enterrement-des-vieux-plugins.html\" title=\"Firefox prépare l'enterrement des vieux plugins\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=200,135\" srcset=\"&#10;                    //c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=200,135 200w,&#10;                                            //c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Firefox prépare l'enterrement des vieux plugins\"/></div>\n        <h4> Firefox prépare l'enterrement des vieux plugins </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/tech/131636-activer-navigation-privee-navigateur-web.html\" title=\"Comment activer la navigation privée sur son navigateur web\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=200,135\" srcset=\"&#10;                    //c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=200,135 200w,&#10;                                            //c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Comment activer la navigation privée sur son navigateur web\"/></div>\n        <h4> Comment activer la navigation privée sur son navigateur web </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/tech/144028-firefox-se-mettra-a-jour-regulierement.html\" title=\"Firefox se mettra à jour un peu moins régulièrement\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=200,135\" srcset=\"&#10;                    //c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=200,135 200w,&#10;                                            //c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Firefox se mettra à jour un peu moins régulièrement\"/></div>\n        <h4> Firefox se mettra à jour un peu moins régulièrement </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article>\n</section>\n",
        "created_at": "2016-04-05T09:07:54+0000",
        "domain_name": "www.numerama.com",
        "id": 1,
        "is_archived": 0,
        "is_starred": 0,
        "language": "fr-FR",
        "mimetype": "text/html",
        "preview_picture": "http://www.numerama.com/content/uploads/2016/04/post-it.jpg",
        "reading_time": 2,
        "tags": [],
        "title": "Le Pocket libre Wallabag fait le plein de fonctionnalités - Tech - Numerama",
        "updated_at": "2016-04-05T09:07:54+0000",
        "url": "http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html",
        "user_email": "",
        "user_id": 1,
        "user_name": "wallabag"
    }

Ora, se eseguite il comando precedente (vedere **Ottenere voci
esistenti**), avrete dei dati.

esempio di cURL:

    curl "https://localhost:8000/api/entries.html?access_token=ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA&url=http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html"

Eliminare una voce
==================

Documentazione per questo metodo:
<http://localhost:8000/api/doc#delete--api-entries-%7Bentry%7D>.{\_format}

    http DELETE http://localhost:8000/api/entries/1.json \
    "Authorization:Bearer ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA"

restituisce

    HTTP/1.1 200 OK
    0: application/json
    Cache-Control: no-cache
    Connection: close
    Content-Type: application/json
    Date: Tue, 05 Apr 2016 09:19:07 GMT
    Host: localhost:8000
    Set-Cookie: PHPSESSID=jopgnfvmuc9a62b27sqm6iulr6; path=/; HttpOnly
    X-Debug-Token: 887cef
    X-Debug-Token-Link: /_profiler/887cef
    X-Powered-By: PHP/7.0.4

    {
        "_links": {
            "self": {
                "href": "/api/entries/"
            }
        },
        "annotations": [],
        "content": "<p class=\"chapo\">Fonctionnant sur le même principe que Pocket, Instapaper ou Readability, le logiciel Wallabag permet de mémoriser des articles pour les lire plus tard. Sa nouvelle version apporte une multitude de nouvelles fonctionnalités.</p><p>Si vous utilisez Firefox comme navigateur web, vous avez peut-être constaté l’arrivée d’<a href=\"http://www.numerama.com/magazine/33292-update-firefox.html\">une fonctionnalité intitulée Pocket</a>. Disponible autrefois sous la forme d’un module complémentaire, et sous un autre nom (Read it Later), elle est depuis le mois de juin 2015 directement incluse au sein de Firefox.</p>\n<p>Concrètement, Pocket sert à garder en mémoire des contenus que vous croisez au fil de la navigation, comme des articles de presse ou des vidéos, afin de pouvoir les consulter plus tard. Pocket fonctionne un peu comme un système de favoris, mais en bien plus élaboré grâce à ses options supplémentaires.</p>\n<p>Mais <a href=\"https://en.wikipedia.org/wiki/Pocket_%28application%29#Firefox_integration\" target=\"_blank\">Pocket fait polémique</a>, car il s’agit d’un projet propriétaire qui est intégré dans un logiciel libre. C’est pour cette raison que des utilisateurs ont choisi de se tourner vers d’autres solutions, comme <strong>Wallabag</strong>, qui est l’équivalent libre de Pocket et d’autres systèmes du même genre, comme Instapaper et Readability.</p>\n<p>Et justement, Wallabag évolue. C’est ce dimanche que la <a href=\"https://www.wallabag.org/blog/2016/04/03/wallabag-v2\" target=\"_blank\">version 2.0.0 du logiciel</a> a été publiée par l’équipe en  charge de son développement et celle-ci contient de nombreux changements par rapport aux moutures précédentes (la <a href=\"http://doc.wallabag.org/fr/v2/\" target=\"_blank\">documentation est traduite</a> en français), lui permettant d’apparaître comme une alternative à Pocket, Instapaper et Readability.</p>\n<p><img class=\"aligncenter size-medium wp-image-160439\" src=\"http://www.numerama.com/content/uploads/2016/04/homepage-680x347.png\" alt=\"homepage\" width=\"680\" height=\"347\" srcset=\"//www.numerama.com/content/uploads/2016/04/homepage-680x347.png 680w, //www.numerama.com/content/uploads/2016/04/homepage-1024x523.png 1024w, //www.numerama.com/content/uploads/2016/04/homepage-270x138.png 270w, //www.numerama.com/content/uploads/2016/04/homepage.png 1286w\" sizes=\"(max-width: 680px) 100vw, 680px\"/></p>\n<p>Parmi les principaux changements que l’on peut retenir avec cette nouvelle version, notons la possibilité d’écrire des annotations dans les articles mémorisés, de filtrer les contenus selon divers critères (temps de lecture, nom de domaine, date de création, statut…), d’assigner des mots-clés aux entrées, de modifier le titre des articles, le support des flux RSS ou encore le support de plusieurs langues dont le français.</p>\n<p>D’autres options sont également à signaler, comme l’aperçu d’un article mémorisé (si l’option est disponible), un guide de démarrage rapide pour les débutants, un outil d’export dans divers formats (PDF, JSON, EPUB, MOBI, XML, CSV et TXT) et, surtout, la possibilité de migrer vers Wallabag depuis Pocket, afin de convaincre les usagers de se lancer.</p>\n    \n    \n    <footer class=\"clearfix\" readability=\"1\"><p class=\"source\">\n        Crédit photo de la une : <a href=\"https://www.flickr.com/photos/bookgrl/2388310523/\">Laura Taylor</a>\n    </p>\n    \n    <p><a href=\"http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html?&amp;show_reader_reports\" target=\"_blank\" rel=\"nofollow\">Signaler une erreur dans le texte</a></p>\n        \n</footer>    <section class=\"related-article\"><header><h3>Articles liés</h3>\n    </header><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/magazine/34444-firefox-prepare-l-enterrement-des-vieux-plugins.html\" title=\"Firefox prépare l'enterrement des vieux plugins\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=200,135\" srcset=\"&#10;                    //c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=200,135 200w,&#10;                                            //c2.lestechnophiles.com/www.numerama.com/content/uploads/2015/10/cimetierecolleville.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Firefox prépare l'enterrement des vieux plugins\"/></div>\n        <h4> Firefox prépare l'enterrement des vieux plugins </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/tech/131636-activer-navigation-privee-navigateur-web.html\" title=\"Comment activer la navigation privée sur son navigateur web\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=200,135\" srcset=\"&#10;                    //c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=200,135 200w,&#10;                                            //c1.lestechnophiles.com/www.numerama.com/content/uploads/2015/11/Incognito.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Comment activer la navigation privée sur son navigateur web\"/></div>\n        <h4> Comment activer la navigation privée sur son navigateur web </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article><article class=\"post-grid format-article\"><a class=\"floatleft\" href=\"http://www.numerama.com/tech/144028-firefox-se-mettra-a-jour-regulierement.html\" title=\"Firefox se mettra à jour un peu moins régulièrement\">\n        <div class=\"cover-preview cover-tech\">\n                            <p>Lire</p>\n            \n                            \n            \n            <img class=\"cover-preview_img\" src=\"http://c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=200,135\" srcset=\"&#10;                    //c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=200,135 200w,&#10;                                            //c0.lestechnophiles.com/www.numerama.com/content/uploads/2016/02/firefox-mobile.jpg?resize=100,67 100w,&#10;                                        \" sizes=\"(min-width: 1001px) 200px, (max-width: 1000px) 100px\" alt=\"Firefox se mettra à jour un peu moins régulièrement\"/></div>\n        <h4> Firefox se mettra à jour un peu moins régulièrement </h4>\n    </a>\n    <footer class=\"span12\">\n    </footer></article>\n</section>\n",
        "created_at": "2016-04-05T09:07:54+0000",
        "domain_name": "www.numerama.com",
        "is_archived": 0,
        "is_starred": 0,
        "language": "fr-FR",
        "mimetype": "text/html",
        "preview_picture": "http://www.numerama.com/content/uploads/2016/04/post-it.jpg",
        "reading_time": 2,
        "tags": [],
        "title": "Le Pocket libre Wallabag fait le plein de fonctionnalités - Tech - Numerama",
        "updated_at": "2016-04-05T09:07:54+0000",
        "url": "http://www.numerama.com/tech/160115-le-pocket-libre-wallabag-fait-le-plein-de-fonctionnalites.html",
        "user_email": "",
        "user_id": 1,
        "user_name": "wallabag"
    }

E se volete elencare le voci esistenti (vedere **Ottenere voci
esistenti**), l'insieme è vuoto.

esempio di cURL:

    curl --request DELETE "https://localhost:8000/api/entries/1.html?access_token=ZGJmNTA2MDdmYTdmNWFiZjcxOWY3MWYyYzkyZDdlNWIzOTU4NWY3NTU1MDFjOTdhMTk2MGI3YjY1ZmI2NzM5MA"

Altri metodi
============

Non scriveremo esempi per ogni metodo API. Guardate l'elenco qui :
<http://localhost:8000/api/doc> per ogni metodo conosciuto.

Risorse di terze parti
======================

Alcune applicazioni o librerie usano le nostre API. Ecco una lista non
esaustiva:

-   [Wrapper Java per l'API di
    wallabag](https://github.com/Strubbl/jWallabag) di Strubbl.
-   [Libreria .NET per l'API di wallabag
    v2](https://github.com/jlnostr/wallabag-api) di Julian Oster.
-   [API di Python per
    wallabag](https://github.com/foxmask/wallabag_api) di FoxMaSk, per
    il suo progetto [Trigger Happy](https://blog.trigger-happy.eu/).
-   [Un plugin](https://github.com/joshp23/ttrss-to-wallabag-v2) creato
    per [Tiny Tiny RSS](https://tt-rss.org/gitlab/fox/tt-rss/wikis/home)
    questo usa l'API di wallabag v2. Di Josh Panter.
-   [Wrapper Golang per l'API di
    wallabag](https://github.com/Strubbl/wallabago) di Strubbl, per il
    suo progetto [wallabag-stats
    graph](https://github.com/Strubbl/wallabag-stats).

