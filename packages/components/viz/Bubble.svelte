<script>
    import { props, config } from '../modules/stores.js';   
    import getSeriesConfig from '../modules/getSeriesConfig.js';
    import getColumnExtents from '../modules/getColumnExtents';
    import formatTitle from '../modules/formatTitle';
    import getCompletedData from '../modules/getCompletedData.js';

    export let y = undefined;
    export let series = undefined;
    export let options = undefined;
    export let size = undefined;
    export let name = undefined; // name to appear in legend (for single series graphics)

    export let shape = undefined;
    export let fillColor = undefined;
    export let opacity = 0.7; // opacity of both fill and outline (ECharts limitation)
    export let outlineColor = undefined;
    export let outlineWidth = undefined;

    export let minSize = 10;
    export let maxSize = 35;

    // Prop check. If local props supplied, use those. Otherwise fall back to global props.
    let data = $props.data;
    let x = $props.x;
    let swapXY = $props.swapXY;
    let xType = $props.xType;
    let xMismatch = $props.xMismatch;
    let columnSummary = $props.columnSummary;
    y = y ?? $props.y;
    series = series ?? $props.series;
    size = size ?? $props.size;
    let yMin = $props.yMin;

    if(!series && typeof y !== 'object'){
        // Single Series
        name = name ?? formatTitle(y, columnSummary[y].title);
    } else {
        // Multi Series
        data = getCompletedData(data, x, y, series);
    }

    // Determine bubble sizes:
    let sizeExtents = getColumnExtents(data, size);
    let dataRange = sizeExtents[1] - sizeExtents[0];
    let minData = sizeExtents[0];
    let minSizeSq;
    let maxSizeSq;
    function bubbleSize(newPoint){
        minSizeSq = Math.pow(minSize, 2);
        maxSizeSq = Math.pow(maxSize, 2);
        let sizeRange = maxSizeSq - minSizeSq;
        
        return Math.sqrt(((newPoint - minData) / dataRange) * sizeRange + minSize)
    }
    

    let baseConfig = {
            type: "scatter",
            label: {
                show: false,
            },
            labelLayout: { hideOverlap: true },
            emphasis: {
                focus: "series",
            },
            symbolSize: function (data) {
                return bubbleSize(data[1]);
            },
            symbol: shape,
            itemStyle: {
                color: fillColor,
                opacity: opacity,
                borderColor: outlineColor,
                borderWidth: outlineWidth
            }
    }

    if(options){
        baseConfig = {...baseConfig, ...options}
    }

    // Overriding global chart config:
    let chartOverrides = {
         yAxis: {
             scale: true,
             boundaryGap: ['1%', '1%']
         },
         xAxis: {
             boundaryGap: [xType === "time" ? '2%' : '1%', '2%']
         }
    }

    let seriesConfig = getSeriesConfig(data, x, y, series, swapXY, baseConfig, name, xMismatch, columnSummary);
    
    config.update(d => {d.series.push(...seriesConfig); return d})

    if(chartOverrides){
        config.update(d => {
            if(swapXY){
                d.yAxis = {...d.yAxis, ...chartOverrides.xAxis};
                d.xAxis = {...d.xAxis, ...chartOverrides.yAxis};
            } else {
                d.yAxis = {...d.yAxis, ...chartOverrides.yAxis};
                d.xAxis = {...d.xAxis, ...chartOverrides.xAxis};
            }
            return d})
    }

</script>