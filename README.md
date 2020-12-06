# Symfony UX: building highly interactive applications by leveraging JavaScript giants¶
  Symfony UX is a series of tools to create a bridge between Symfony and the JavaScript ecosystem. It stands on the shoulders of JavaScript giants: npm, Webpack Encore and Stimulus.

1. Create a symfony 5 project and name it for example: "symfony-javaScript-ecosystem"

    + composer create-project symfony/website-skeleton:"5.2.x@dev" symfony-javaScript-ecosystem

2. Run Symfony Local Web Server  

    + symfony server:start
    OR
    + symfony server:start -d (démarrer le serveur en background) 

3. To use Symfony UX, first update your Symfony Flex and Webpack Encore dependencies:

    + composer update symfony/flex
    OR
    + yarn upgrade "@symfony/webpack-encore@^0.32.0"

4. Symfony Flex will react to every PHP package you install that contains JavaScript code. For example, you can now install the Chart.js component:

    + composer require symfony/ux-chartjs

5. Symfony Flex has just synchronized the package.json file of your project with the Chartjs PHP package that you installed. You will now find in this file(package.json) a new JavaScript module.

Symfony Flex has also updated a new assets / controllers.json file in your project. This file references all the Stimulus controllers provided by the installed Symfony UX packages to allow Webpack Encore to add them to your created JavaScript files.

6. Due to these changes, you should now install the new JavaScript dependencies and compile the new files:

    + yarn install --force
    + yarn encore dev

7. That's it! By leveraging Symfony Flex, Symfony UX Chart.js has been installed and configured both as a Symfony bundle in PHP and as a JavaScript library compiled into your application's built-in files.

This means that you can now create a chart using the bundle.

8. Create your Controller with the command make:controller : 

    + symfony console make:controller HomeController

/***HomeController***/ 




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



9. Once created in PHP, a chart can be displayed using Twig:

{{ render_chart(chart) }}