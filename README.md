# Symfony UX: building highly interactive applications by leveraging JavaScript giants¶
  Symfony UX is a series of tools to create a bridge between Symfony and the JavaScript ecosystem. It stands on the shoulders of JavaScript giants: npm, Webpack Encore and Stimulus.

1. Créez un projet symfony 5 et nommez-le "symfony-javaScript-ecosystem"

    + composer create-project symfony/website-skeleton:"5.2.x@dev" symfony-javaScript-ecosystem
    + symfony server:start
    OU
    + symfony server:start -d (démarrer le serveur en background) 

2. Pour utiliser Symfony UX, mettez d'abord à jour vos dépendances Symfony Flex et Webpack Encore:

    + composer update symfony/flex
    OU
    + yarn upgrade "@symfony/webpack-encore@^0.32.0"

3. Symfony Flex réagira à chaque package PHP que vous installez contenant du code JavaScript. Par exemple, vous pouvez maintenant installer le composant Chart.js:

    + composer require symfony/ux-chartjs

4. Symfony Flex vient de synchroniser le fichier package.json de votre projet avec le package PHP Chartjs  que vous avez installé. Vous trouverez maintenant dans ce fichier un nouveau module JavaScript.

Symfony Flex a également mis à jour un nouveau fichier assets / controllers.json dans votre projet. Ce fichier référence tous les contrôleurs Stimulus fournis par les packages Symfony UX installés pour permettre à Webpack Encore de les ajouter à vos fichiers JavaScript créés.

5. En raison de ces modifications, vous devez maintenant installer les nouvelles dépendances JavaScript et compiler les nouveaux fichiers:

    + yarn install --force
    + yarn encore dev

6. c'est ça! En exploitant Symfony Flex, Symfony UX Chart.js a été installé et configuré à la fois en tant que bundle Symfony en PHP et en tant que bibliothèque JavaScript compilée dans les fichiers intégrés de votre application.

Cela signifie que vous pouvez maintenant créer un graphique à l'aide du bundle.

7. Créez votre  Controller avec la commande make:controller : 

    + symfony console make:controller HomeController

/*******HomeController********/ 

use Symfony\UX\Chartjs\Builder\ChartBuilderInterface;

use Symfony\UX\Chartjs\Model\Chart;

class HomeController extends AbstractController

{

    /**
     * @Route("/", name="homepage")
     */

    public function index(ChartBuilderInterface $chartBuilder): Response
    {
        $chart = $chartBuilder->createChart(Chart::TYPE_LINE);

        $chart->setData([
            'labels' => ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
            'datasets' => [
                [
                    'label' => 'My First dataset',
                    'backgroundColor' => 'rgb(255, 99, 132)',
                    'borderColor' => 'rgb(255, 99, 132)',
                    'data' => [0, 10, 5, 2, 20, 30, 45],
                ],
            ],
        ]);

        $chart->setOptions([/* ... */]);

        return $this->render('home/index.html.twig', [
            'chart' => $chart,
        ]);
    }
}

8. Une fois créé en PHP, un graphique peut être affiché en utilisant Twig:

{{ render_chart(chart) }}