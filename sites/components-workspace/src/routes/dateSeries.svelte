<script>
    import LineChart from '$lib/viz/LineChart.svelte'
    import AreaChart from '$lib/viz/AreaChart.svelte'
    import BubbleChart from '$lib/viz/BubbleChart.svelte'
    import BarChart from '$lib/viz/BarChart.svelte'
    import Histogram from '$lib/viz/Histogram.svelte'
    import ScatterPlot from '$lib/viz/ScatterPlot.svelte'
    import DataTable from '$lib/viz/DataTable.svelte'
    import Value from '$lib/viz/Value.svelte'
    import {tidy, complete} from "@tidyjs/tidy";

    import Line from '$lib/viz/Line.svelte'
    import Bar from '$lib/viz/Bar.svelte'
    import Scatter from '$lib/viz/Scatter.svelte'
    import Area from '$lib/viz/Area.svelte'

    import Chart from '$lib/viz/Chart.svelte'



let orig = [
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'NY', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2020-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2021-01-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-01-01', banks: 1}
]


let fullMonths = [
    {fed_reserve_district: 'SF', established_date: '2017-11-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-10-15', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-11-01', banks: 3},
    {fed_reserve_district: 'ATL', established_date: '2017-11-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2017-12-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2017-11-', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-12-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-12-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2017-12-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 2},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2018-03-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2018-03-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2018-04-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2018-04-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2018-05-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2018-05-01', banks: 1}
]

let nulls = [
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2015-01-01', banks: null},
    {fed_reserve_district: 'ATL', established_date: '2015-01-01', banks: null},
    {fed_reserve_district: 'DAL', established_date: '2015-01-01', banks: null},
    {fed_reserve_district: 'KC', established_date: '2015-01-01', banks: null},
    {fed_reserve_district: 'CHI', established_date: '2015-01-01', banks: null},
    
    {fed_reserve_district: 'NY', established_date: '2016-01-01', banks: null},
    {fed_reserve_district: 'SF', established_date: '2016-01-01', banks: null},
    {fed_reserve_district: 'ATL', established_date: '2016-01-01', banks: null},
    {fed_reserve_district: 'DAL', established_date: '2016-01-01', banks: null},
    {fed_reserve_district: 'KC', established_date: '2016-01-01', banks: null},
    {fed_reserve_district: 'CHI', established_date: '2016-01-01', banks: null},

    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'KC', established_date: '2017-01-01', banks: null},
    {fed_reserve_district: 'CHI', established_date: '2017-01-01', banks: null},
    {fed_reserve_district: 'NY', established_date: '2017-01-01', banks: null},

    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: null},
    {fed_reserve_district: 'KC', established_date: '2018-01-01', banks: null},

    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'KC', established_date: '2019-01-01', banks: null},

    {fed_reserve_district: 'NY', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2020-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2020-01-01', banks: null},
    {fed_reserve_district: 'DAL', established_date: '2020-01-01', banks: null},

    {fed_reserve_district: 'SF', established_date: '2021-01-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2021-01-01', banks: null},
    {fed_reserve_district: 'NY', established_date: '2021-01-01', banks: null}
]

let full = [
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2015-01-01', banks: 0},
    
    {fed_reserve_district: 'NY', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'SF', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2016-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'KC', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2017-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2018-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'KC', established_date: '2019-01-01', banks: 0},

    {fed_reserve_district: 'NY', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2020-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2020-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2020-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2021-02-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-02-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-02-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-02-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2021-02-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2021-02-01', banks: 0}
]

let xSync = [
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2015-01-01', banks: 0},
    
    {fed_reserve_district: 'NY', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'SF', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2016-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'KC', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2017-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2018-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'KC', established_date: '2019-01-01', banks: 0},

    {fed_reserve_district: 'NY', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2020-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2020-01-31', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2020-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2020-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2021-01-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2021-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2021-01-01', banks: 0}
]

let missingX = [ // take out 2016 and 2020
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2015-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'KC', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2017-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'SF', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2018-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'KC', established_date: '2019-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2021-01-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2021-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2021-01-01', banks: 0}
]

let missingY = [ // take out SF 2018 and NY 2016
    {fed_reserve_district: 'NY', established_date: '2015-01-01', banks: 1},
    {fed_reserve_district: 'SF', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2015-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2015-01-01', banks: 0},
    
    {fed_reserve_district: 'SF', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'ATL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2016-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2016-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2017-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2017-01-01', banks: 3},
    {fed_reserve_district: 'KC', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'CHI', established_date: '2017-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2017-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2018-01-01', banks: 3},
    {fed_reserve_district: 'NY', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2018-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2018-01-01', banks: 0},
    {fed_reserve_district: 'KC', established_date: '2018-01-01', banks: 0},

    {fed_reserve_district: 'ATL', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'NY', established_date: '2019-01-01', banks: 4},
    {fed_reserve_district: 'CHI', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'SF', established_date: '2019-01-01', banks: 1},
    {fed_reserve_district: 'DAL', established_date: '2019-01-01', banks: 2},
    {fed_reserve_district: 'KC', established_date: '2019-01-01', banks: 0},

    {fed_reserve_district: 'NY', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'ATL', established_date: '2020-01-01', banks: 4},
    {fed_reserve_district: 'SF', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2020-01-01', banks: 1},
    {fed_reserve_district: 'CHI', established_date: '2020-01-01', banks: 0},
    {fed_reserve_district: 'DAL', established_date: '2020-01-01', banks: 0},

    {fed_reserve_district: 'SF', established_date: '2021-01-01', banks: 2},
    {fed_reserve_district: 'ATL', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'CHI', established_date: '2021-01-01', banks: 3},
    {fed_reserve_district: 'DAL', established_date: '2021-01-01', banks: 1},
    {fed_reserve_district: 'KC', established_date: '2021-01-01', banks: 0},
    {fed_reserve_district: 'NY', established_date: '2021-01-01', banks: 0}
]
let filling = tidy(missingY, complete(['fed_reserve_district', 'established_date']))


let titles  = [
    "Full Data",
    "Missing Y",
    "Missing X",
    "X out of Sync",
    "Nulls"
]
 </script>

<h1>Series Column with Date X Axis</h1>
<h2>Line Chart</h2>
<LineChart data={full} series=fed_reserve_district x=established_date title={titles[0]} markers=true/>
<LineChart data={missingY} series=fed_reserve_district x=established_date title={titles[1]} markers=true/>
<LineChart data={filling} series=fed_reserve_district x=established_date title={"Attempted Fill from Missing Y dataset"}/>
<LineChart data={missingX} series=fed_reserve_district x=established_date title={titles[2]}/>
<LineChart data={xSync} series=fed_reserve_district x=established_date title={titles[3]}/>
<LineChart data={nulls} series=fed_reserve_district x=established_date title={titles[4]}/>

<h2>Area Chart</h2>
<AreaChart data={full} series=fed_reserve_district x=established_date title={titles[0]}/>
<AreaChart data={missingY} series=fed_reserve_district x=established_date title={titles[1]}/>
<AreaChart data={filling} series=fed_reserve_district x=established_date title={"Attempted Fill from Missing Y dataset"}/>
<AreaChart data={missingX} series=fed_reserve_district x=established_date title={titles[2]}/>
<AreaChart data={xSync} series=fed_reserve_district x=established_date title={titles[3]}/>
<AreaChart data={nulls} series=fed_reserve_district x=established_date title={titles[4]}/>

<h2>Stacked Bar Chart</h2>
<BarChart data={full} series=fed_reserve_district x=established_date title={titles[0]}/>
<BarChart data={missingY} series=fed_reserve_district x=established_date title={titles[1]}/>
<BarChart data={missingX} series=fed_reserve_district x=established_date title={titles[2]}/>
<BarChart data={xSync} series=fed_reserve_district x=established_date title={titles[3]}/>
<BarChart data={nulls} series=fed_reserve_district x=established_date title={titles[4]}/>

<h2>Horizontal Stacked Bar Chart</h2>
<BarChart data={full} x=established_date series=fed_reserve_district swapXY=true title="Full Data" sort=false/>
<BarChart data={missingY} x=established_date series=fed_reserve_district swapXY=true title="Missing Y"/>
<BarChart data={missingX} x=established_date series=fed_reserve_district swapXY=true title="Missing X"/>
<BarChart data={xSync} x=established_date series=fed_reserve_district swapXY=true title="X out of sync"/>
<BarChart data={nulls} x=established_date series=fed_reserve_district swapXY=true title="Nulls"/>

<h2>Grouped Bar Chart</h2>
<BarChart data={full} series=fed_reserve_district type=grouped x=established_date title={titles[0]}/>
<BarChart data={missingY} series=fed_reserve_district type=grouped x=established_date title={titles[1]}/>
<BarChart data={missingX} series=fed_reserve_district type=grouped x=established_date title={titles[2]}/>
<BarChart data={xSync} series=fed_reserve_district type=grouped x=established_date title={titles[3]}/>
<BarChart data={nulls} series=fed_reserve_district type=grouped x=established_date title={titles[4]}/>

<h2>Horizontal Grouped Bar Chart</h2>
<BarChart data={full} series=fed_reserve_district x=established_date swapXY=true type=grouped title="Full Data"/>
<BarChart data={missingY} series=fed_reserve_district x=established_date swapXY=true type=grouped title="Missing Y"/>
<BarChart data={missingX} series=fed_reserve_district x=established_date swapXY=true type=grouped title="Missing X"/>
<BarChart data={xSync} series=fed_reserve_district x=established_date swapXY=true type=grouped title="X out of sync"/>
<BarChart data={nulls} series=fed_reserve_district x=established_date swapXY=true type=grouped title="Nulls"/>

<h2>Scatter Plot</h2>
<ScatterPlot data={full} series=fed_reserve_district x=established_date title={titles[0]}/>
<ScatterPlot data={missingY} series=fed_reserve_district x=established_date title={titles[1]}/>
<ScatterPlot data={missingX} series=fed_reserve_district x=established_date title={titles[2]}/>
<ScatterPlot data={xSync} series=fed_reserve_district x=established_date title={titles[3]}/>
<ScatterPlot data={nulls} series=fed_reserve_district x=established_date title={titles[4]}/>

<h2>Bubble Chart</h2>
<BubbleChart data={full} series=fed_reserve_district size=banks y=banks x=established_date title={titles[0]}/>
<BubbleChart data={missingY} series=fed_reserve_district size=banks y=banks x=established_date title={titles[1]}/>
<BubbleChart data={missingX} series=fed_reserve_district size=banks y=banks x=established_date title={titles[2]}/>
<BubbleChart data={xSync} series=fed_reserve_district size=banks y=banks x=established_date title={titles[3]}/>
<BubbleChart data={nulls} series=fed_reserve_district size=banks y=banks x=established_date title={titles[4]}/>
