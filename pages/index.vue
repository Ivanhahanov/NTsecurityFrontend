<template>
  <div>

    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>NTSecurity</title>
    <meta name="description" content="description here">
    <meta name="keywords" content="keywords,here">

    <link href="https://fonts.googleapis.com/css?family=Nunito:400,700,800" rel="stylesheet">

    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.3.1/css/all.css"
          integrity="sha384-mzrmE5qonljUremFsqc01SB46JvROS7bZs3IO2EmfFsd15uHvIt+Y8vEf7N7fWAU" crossorigin="anonymous">
    <link href="https://unpkg.com/tailwindcss/dist/tailwind.min.css" rel="stylesheet">
    <!--Replace with your tailwind.css once created-->


    <div class="flex h-screen bg-gray-100 font-sans">
      <div class="flex flex-row flex-wrap flex-1 flex-grow content-start">
        <div class="h-40 lg:h-20 w-full flex flex-wrap">
          <nav id="header"
               class="bg-gray-200 w-full lg:max-w-sm flex items-center border-b-1 border-gray-300 order-2 lg:order-1">
            <div class="px-2 w-full">
              <select name=""
                      class="bg-gray-300 border-2 border-gray-200 rounded-full w-full py-3 px-4 text-gray-500 font-bold leading-tight focus:outline-none focus:bg-white focus:shadow-md"
                      id="form-field2">
                <option value="Default">default</option>
                <option value="A">report a</option>
                <option value="B">report b</option>
                <option value="C">report c</option>
              </select>
            </div>
          </nav>
          <nav id="header1" class="bg-gray-100 w-auto flex-1 border-b-1 border-gray-300 order-1 lg:order-2">

            <div class="flex h-full justify-between items-center">

              <!--Search-->
              <div class="relative w-full max-w-3xl px-6">
                <div class="absolute h-10 mt-1 left-0 top-0 flex items-center pl-10">
                  <svg class="h-4 w-4 fill-current text-gray-600" xmlns="http://www.w3.org/2000/svg"
                       viewBox="0 0 20 20">
                    <path
                        d="M12.9 14.32a8 8 0 1 1 1.41-1.41l5.35 5.33-1.42 1.42-5.33-5.34zM8 14A6 6 0 1 0 8 2a6 6 0 0 0 0 12z"></path>
                  </svg>
                </div>

                <input id="search-toggle" type="search" placeholder="search"
                       class="block w-full bg-gray-200 focus:outline-none focus:bg-white focus:shadow-md text-gray-700 font-bold rounded-full pl-12 pr-4 py-3"
                       onkeyup="updateSearchResults(this.value);">

              </div>
              <!-- / Search-->

              <!--Menu-->

              <div class="flex relative inline-block pr-6">

                <div class="relative text-sm">
                  Menu
                </div>

              </div>

              <!-- / Menu -->

            </div>

          </nav>
        </div>

        <!--Dash Content -->
        <div id="dash-content" class="bg-gray-200 py-6 lg:py-0 w-full lg:max-w-sm flex flex-wrap content-start">

          <div class="w-1/2 lg:w-full" @click="onPickFile">
            <input
                type="file"
                style="display: none"
                ref="fileInput"
                accept="*"
                @change="onFilePicked"/>
            <div
                class="border-2 border-gray-400 border-dashed hover:border-transparent hover:bg-white hover:shadow-xl rounded p-6 m-2 md:mx-10 md:my-6">
              <div class="flex flex-col items-center">
                <div class="flex-shrink pr-4">
                  <div class="rounded-full p-3 bg-gray-300"><i class="fa fa-file fa-fw fa-inverse text-indigo-500"></i>
                  </div>
                </div>
                <div class="flex-1">
                  <h3 class="font-bold text-3xl align-center">Upload</h3>
                  <h5 class="font-bold text-gray-500">upload file</h5>
                </div>
              </div>

            </div>

          </div>

          <div v-for="item in cards" class="w-1/2 lg:w-full">
            <div v-on:click="click_menu(item.item_event)"
                 class="border-2 border-gray-400 border-dashed hover:border-transparent hover:bg-white hover:shadow-xl rounded p-6 m-2 md:mx-10 md:my-6">

              <div class="flex flex-col items-center">
                <div class="flex-shrink pr-4">
                  <div class="rounded-full p-3 bg-gray-300"><i
                      :class="item.icon" class="fa fa-fw fa-inverse text-indigo-500"></i></div>
                </div>
                <div class="flex-1">
                  <h3 class="font-bold text-3xl align-center">{{ item.name }}</h3>
                  <h5 class="font-bold text-gray-500">{{ item.description }}</h5>
                </div>
              </div>
            </div>
          </div>

        </div>

        <!--Graph Content -->
        <div id="main-content" class="w-full flex-1">
          <div class="flex flex-1 flex-wrap">
            <div class="w-full xl:w-2/3 p-6 xl:max-w-6xl">
              <!--"Container" for the graphs"-->
              <div class="max-w-full lg:max-w-3xl xl:max-w-5xl">
                <div>
                  <div v-if="Upload">

                    <v-alert
                        border="top"
                        colored-border
                        type="info"
                        elevation="2"
                    >
                      File Uploaded
                    </v-alert>

                  </div>
                  <div v-if="Scan">

                    <v-alert
                        border="top"
                        colored-border
                        type="info"
                        elevation="2"
                    >
                      Start scan
                    </v-alert>

                  </div>
                  <div v-if="Detected">

                    <v-alert
                        border="top"
                        colored-border
                        type="warning"
                        elevation="2"
                    >
                      Malware Detected
                    </v-alert>

                  </div>
                </div>

                <!--Graph Card-->
                <div class="border-b p-3">
                  <h5 class="font-bold text-black">Detect Statistic</h5>
                </div>
                <div class="p-5">
                  <div class="container">
                    <bar-chart :data="barChartData" :options="barChartOptions" :height="200"/>
                  </div>
                </div>

                <!--/Graph Card-->

                <!--Table Card-->
                <div class="p-3">
                  <div class="border-b p-3">
                    <h5 class="font-bold text-black">Latest Results</h5>
                  </div>
                  <v-data-table
                      :headers="headers"
                      :items="list"
                      :items-per-page="5"
                      class="elevation-1"
                  ></v-data-table>
                  <p class="py-2"><a href="#">See More issues...</a></p>
                </div>
                <!--/table Card-->
              </div>
            </div>

            <div class="w-full xl:w-1/3 p-6 xl:max-w-4xl border-l-1 border-gray-300">

              <!--"Container" for the graphs"-->
              <div class="max-w-sm lg:max-w-3xl xl:max-w-5xl">

                <!--Graph Card-->

                <div class="border-b p-3">
                  <h5 class="font-bold text-black">Graph</h5>
                </div>
                <div class="p-5">
                  <div class="ct-chart ct-golden-section" id="chart2"></div>
                </div>

                <!--/Graph Card-->

                <!--Graph Card-->
                <div class="border-b p-3">
                  <h5 class="font-bold text-black">Graph</h5>
                </div>
                <div class="p-5">
                  <div class="ct-chart ct-golden-section" id="chart3"></div>
                </div>

                <!--/Graph Card-->

                <!--Graph Card-->

                <div class="border-b p-3">
                  <h5 class="font-bold text-black">Graph</h5>
                </div>
                <div class="p-5">
                  <div class="ct-chart ct-golden-section" id="chart4"></div>
                </div>

                <!--/Graph Card-->

                <!--Template Card-->
                <div class="p-3">
                  <div class="border-b p-3">
                    <h5 class="font-bold text-black">Template</h5>
                  </div>
                  <div class="p-5">

                  </div>
                </div>
                <!--/Template Card-->
              </div>
            </div>
          </div>
        </div>

      </div>

      <script src="https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js"></script>
      <script>
        /* Refer to https://gionkunz.github.io/chartist-js/examples.html for setting up the graphs */


        var chartScatter = new Chartist.Line('#chart2', {
          labels: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12'],
          series: [
            [12, 9, 7, 8, 5, 4, 6, 2, 3, 3, 4, 6],
            [4, 5, 3, 7, 3, 5, 5, 3, 4, 4, 5, 5],
            [5, 3, 4, 5, 6, 3, 3, 4, 5, 6, 3, 4],
            [3, 4, 5, 6, 7, 6, 4, 5, 6, 7, 6, 3]
          ]
        }, {
          low: 0
        });

        chartScatter.on('draw', function (data) {
          if (data.type === 'line' || data.type === 'area') {
            data.element.animate({
              d: {
                begin: 500 * data.index,
                dur: 1000,
                from: data.path.clone().scale(1, 0).translate(0, data.chartRect.height()).stringify(),
                to: data.path.clone().stringify(),
                easing: Chartist.Svg.Easing.easeOutQuint
              }
            });
          }
        });

        var chartBar = new Chartist.Bar('#chart3', {
          labels: ['Q1', 'Q2', 'Q3', 'Q4'],
          series: [
            [800000, 1200000, 1400000, 1300000],
            [200000, 400000, 500000, 300000],
            [100000, 200000, 400000, 600000]
          ]
        }, {
          stackBars: true,
          axisY: {
            labelInterpolationFnc: function (value) {
              return (value / 1000) + 'k';
            }
          }
        })

        chartBar.on('draw', function (data) {
          if (data.type === 'bar') {
            data.element.attr({
              style: 'stroke-width: 30px'
            }),
                data.element.animate({
                  y2: {
                    dur: '0.5s',
                    from: data.y1,
                    to: data.y2
                  }
                });
          }
        });

        var chartPie = new Chartist.Pie('#chart4', {
          series: [10, 20, 50, 20, 5, 50, 15],
          labels: [1, 2, 3, 4, 5, 6, 7]
        }, {
          donut: true,
          showLabel: true
        });

        chartPie.on('draw', function (data) {
          if (data.type === 'slice') {
            var pathLength = data.element._node.getTotalLength();
            data.element.attr({
              'stroke-dasharray': pathLength + 'px ' + pathLength + 'px'
            });

            var animationDefinition = {
              'stroke-dashoffset': {
                id: 'anim' + data.index,
                dur: 200,
                from: -pathLength + 'px',
                to: '0px',
                easing: Chartist.Svg.Easing.easeOutQuint,
                fill: 'freeze'
              }
            };

            if (data.index !== 0) {
              animationDefinition['stroke-dashoffset'].begin = 'anim' + (data.index - 1) + '.end';
            }

            data.element.attr({
              'stroke-dashoffset': -pathLength + 'px'
            });

            data.element.animate(animationDefinition, false);
          }
        });
      </script>

      <script>
        /*Toggle dropdown list*/
        /*https://gist.github.com/slavapas/593e8e50cf4cc16ac972afcbad4f70c8*/

        var userMenuDiv = document.getElementById("userMenu");
        var userMenu = document.getElementById("userButton");

        document.onclick = check;

        function check(e) {
          var target = (e && e.target) || (event && event.srcElement);

          //User Menu
          if (!checkParent(target, userMenuDiv)) {
            // click NOT on the menu
            if (checkParent(target, userMenu)) {
              // click on the link
              if (userMenuDiv.classList.contains("invisible")) {
                userMenuDiv.classList.remove("invisible");
              } else {
                userMenuDiv.classList.add("invisible");
              }
            } else {
              // click both outside link and outside menu, hide menu
              userMenuDiv.classList.add("invisible");
            }
          }

        }

        function checkParent(t, elm) {
          while (t.parentNode) {
            if (t == elm) {
              return true;
            }
            t = t.parentNode;
          }
          return false;
        }
      </script>

    </div>
  </div>
</template>

<script>
import Header from '~/components/Header'
import axios from '@nuxtjs/axios'
import BarChart from '~/components/BarChart'

const chartColors = {
  red: 'rgb(255, 99, 132)',
  orange: 'rgb(255, 159, 64)',
  yellow: 'rgb(255, 205, 86)',
  green: 'rgb(75, 192, 192)',
  blue: 'rgb(54, 162, 235)',
  purple: 'rgb(153, 102, 255)',
  grey: 'rgb(201, 203, 207)'
};

export default {
  components: {
    Header,
    BarChart,
  },

  head: {
    script: [
      {
        src: 'https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.js'
      }
    ],
    link: [
      {
        rel: 'stylesheet',
        href: 'https://cdn.jsdelivr.net/chartist.js/latest/chartist.min.css'
      }
    ]
  },
  data() {
    return {
      someBoolean: false,
      Detected: false,
      Scan: false,
      Upload: false,
      cards: [

        {name: "Search", description: "Start scanning", icon: "fa-bug", item_event: "search"},
        {name: "30 Files", description: "Total review", icon: "fa-users", item_event: ""},
        {name: "3 Days", description: "Server Uptime", icon: "fa-server", item_event: ""},
      ],
      total_file_checked: '25',
      barChartData: {
        labels: ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15',
          '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29', '30'],
        datasets: [
          {
            label: 'Income',
            // backgroundColor: ["red", "orange", "yellow"],
            backgroundColor: [chartColors.yellow, '', chartColors.yellow, chartColors.orange,
              chartColors.orange, chartColors.yellow, chartColors.yellow, chartColors.orange, '',
              '', chartColors.orange, '', chartColors.orange, chartColors.orange,
              chartColors.orange, chartColors.red, chartColors.yellow, chartColors.yellow, chartColors.yellow,
              chartColors.orange,],
            data: [1, 0, 1, 3, 3, 1, 1, 2, 0, 0, 0, 3, 0, 3, 3, 4, 1, 0, 1, 2]
          }
        ]
      },
      barChartOptions: {
        responsive: true,
        legend: {
          display: false,
        },
        title: {
          display: true,
          text: 'Monthly Income'
        },
        scales: {
          yAxes: [
            {
              ticks: {
                beginAtZero: true
              }
            }
          ]
        }
      },
      answer: '',
      headers: [
        {text: "Source", value: "source"},
        {
          text: 'Filename', value: 'filename',
          align: 'start',
          sortable: false,
        },
        {text: 'Result', value: 'result'},
        {text: 'Info', value: 'info'},
      ],
      list: [
        {
          source: "91.201.41.138",
          filename: 'Hello World.rtf ',
          result: 'Malware founded',
          info: 'Founded CVE-2017-8759',

        },
        {
          source: "212.11.155.167",
          filename: 'Contract.pdf',
          result: 'No malware found',
          info: '',

        }]
    };
  },
  methods: {
    click_menu: function (item_event) {
      console.log('click' + item_event)
    },
    onPickFile() {
      this.$refs.fileInput.click()
    },
    onFilePicked(event) {
      //     const files = event.target.files
      //     let formData = new FormData();
      //     formData.append('file', files[0]);
      //     axios.post('http://localhost/file/upload_file/',
      //         formData, {
      //           header: {
      //             "accept": "application/json",
      //             "Content-Type": "multipart/form-data",
      //           }
      //         }
      //     ).then(function () {
      //       console.log('SUCCESS!!')
      //
      //     })
      //         .catch(function () {
      //           console.log('FAILURE!!');
      //         });
      //
      setTimeout(() => {
        this.Upload = true;
      }, 1000);
      setTimeout(() => {
        this.Scan = true;
        this.Upload = false;
      }, 2000);
      setTimeout(() => {
        this.Detected = true;
        this.Scan = false;
      }, 3500);
      setTimeout(() => {
        this.Detected = false;
        this.list = [
          {
            source: "193.41.140.35",
            filename: 'Report.rtf',
            result: 'Malware founded',
            info: 'Founded CVE-2017-11882',

          },
          {
            source: "91.201.41.138",
            filename: 'Hello World.rtf ',
            result: 'Malware founded',
            info: 'Founded CVE-2017-8759',

          },
          {
            source: "212.11.155.167",
            filename: 'Contract.pdf',
            result: 'No malware found',
            info: '',

          }];
        this.cards = [

          {name: "Search", description: "Start scanning", icon: "fa-bug", item_event: "search"},
          {name: "31 Files", description: "Total review", icon: "fa-users", item_event: ""},
          {name: "3 Days", description: "Server Uptime", icon: "fa-server", item_event: ""},
        ]
      }, 5000);
    },

  }

}
</script>

<style>
.nunito {
  font-family: 'nunito', font-sans;
}

.border-b-1 {
  border-bottom-width: 1px;
}

.border-l-1 {
  border-left-width: 1px;
}

hover\:border-none:hover {
  border-style: none;
}

#sidebar {
  transition: ease-in-out all .3s;
  z-index: 9999;
}

#sidebar span {
  opacity: 0;
  position: absolute;
  transition: ease-in-out all .1s;
}

#sidebar:hover {
  width: 150px;
  box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
  /*shadow-2xl*/
}

#sidebar:hover span {
  opacity: 1;
}
</style>

