<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <img src="../assets/logo.png" id="img1" style="display:none;">
    <div style="text-align:left; padding-bottom:10px;">
      <h3>
        do some operate:
      </h3>
      <button @click="modifytoredcircle">modifytoredcircle</button>
      <button @click="createGroupElm">createGroupElm</button>
      <button @click="createLable">createLable</button>
      <button @click="pingyi">平移圆形</button>
    </div>
    <canvas id="canvasBox" style="border:1px solid red; width:300px; height:200px;">
      
    </canvas>

  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to vue yizheng!134455667788900',
      cbox: null,
      rect: null,
      faX: 100,
      faY: 50,
      firstX: 0,
      firstY: 0,
      yuan: null,
      nowTarget: null,
      triangle:null,
      point: null
    }
  },
  mounted(){
    console.log('fabric:',fabric);


    var that = this;

    this.cbox = new fabric.Canvas('canvasBox',{
      width: 1000,
      height: 300,
      backgroundColor: '#eee'
    });

    this.rect = new fabric.Rect({
      name:'father1',
      left: 100,
      top:50,
      width:300,
      height:200,
      fill:'green',
      hasControls: true,
      hasBorders: true,
      opacity: 1
    })

    this.yuan  = new fabric.Circle({
      name:'son1',
      left: this.faX + 59,
      top: this.faY + 10,
      radius: 30,
      hasControls: true,
      hasBorders: true,
      fill: 'red' 
    })

    this.triangle = new fabric.Triangle({
      width: 30,
      height: 50,
      fill: 'yellow',
      top: 100,
      left: 220
    })


    // this.cbox.add(this.rect);
    this.cbox.add(this.yuan);
    this.cbox.add(this.triangle);
    console.log('allObj:',this.cbox.getObjects());

    // Events
    // var canvas = new fabric.Canvas('...');
    /*this.cbox.on('mouse:down', function(options) {
      console.log(options.e.clientX, options.e.clientY);
    });*/

    this.rect.on('moving',function(opt){
      console.log('1111111object--moving:')
    })
    this.rect.on('moved',function(opt){
      console.log('222222object--moved:')
    })
    this.rect.on('scaling',function(opt){
      console.log('33333object--scaling:')
    })

    // this.cbox.on('mouse:down', function(options) {
    //   console.log('---mouse:move12--',options);
    //   if (options.target && options.target.name && options.target.name.includes('father')) {
    //     // console.log('---typeOfModules---:',options.target.type);
    //     // console.log('options-down:',options);
    //     // that.firstX = options.target.left;
    //     // that.firstY = options.target.top;
    //     // console.log('the gap:',that.firstX, that.firstY);

    //     // // console.log('this.yuan',that.yuan,options.target==that.rect);
    //     // that.nowTarget = options.target;
    //   }
    // });

    // this.cbox.on('object:moving', function(e) {      
      
    //   // 思路1.相对位置-实现整体移动(计算量最大)
    //   console.log('---object:move--be invoked12--',e);
    //   if(e.target.name && e.target.name.includes('father')){
    //     console.log('name:',e.target.name);
    //     console.log('left,top:',e.target.left,e.target.top,that.faX,that.faY)

    //     that.faX = e.target.left;
    //     that.faY = e.target.top;

    //     var extraX = e.target.left - that.firstX;
    //     var extraY = e.target.top - that.firstY;
    //     console.log('later:',extraX,extraY);
    //     // that.yuan.set('left',that.yuan.left+extraX);
    //     // that.yuan.set('top', that.yuan.top+extraY);
    //     // that.cbox.renderAll();

    //     // that.cbox.moveTo(that.nowTarget,0)
    //     // that.cbox.moveTo(that.yuan,9)

    //     // 
    //     that.firstX = e.target.left;
    //     that.firstY = e.target.top;
    //   }

    //   // 思路2.实现容器功能，外层移动时，内部元素没变化。整体移动(计算量最小-难度大)。

    // })

    this.cbox.on('selection:created', function(options) {
      console.log('selected-create')
    })

    this.cbox.on('mouse:up', function(options) {
      console.log('mouse:up');
      // console.log('options.target && that.nowTarget',options.target,that.nowTarget)
      if(!options.target && that.nowTarget && that.nowTarget.name.includes('father')){
        // that.nowTarget.set('opacity',1);
        console.log('name-mouseUp:',that.nowTarget.name)
        
      }else{
        that.nowTarget = null;
      }
      
      // that.cbox.renderAll();

      // that.cbox.moveTo(that.nowTarget,0)
      // that.cbox.moveTo(that.yuan,9)
      
    })

    this.cbox.on('selection:cleared', function(options) {
      console.log('selection:cleared--55',options);
      // that.cbox.renderAll();
    })


  },
  methods: {
    Move (x, y) {
      // 
      var delta = new fabric.Point(x, y);
      this.canvas.relativePan(delta);
    },
    MoveToPoint (x, y) {
      this.set('left', x)
      this.set('top', y)
      
    },
    pingyi(){

      this.yuan.set('left', 500);
      this.cbox.renderAll();
      
    },
    createLable(){

        var LabeledRect = fabric.util.createClass(
          fabric.Rect, 
          {
            type: 'labeledRect',
            initialize: function(options) {
              options || (options = { });

              this.callSuper('initialize', options);
              this.set('label', options.label || '');
            },
            toObject: function() {
              return fabric.util.object.extend(
                this.callSuper('toObject'), 
                {
                  label: this.get('label')
                }
              );
          },
          _render: function(ctx) {
            this.callSuper('_render', ctx);

            ctx.font = '20px Helvetica';
            ctx.fillStyle = '#333';
            console.log('-this.width/2, -this.height/2 + 20:',-this.width/2, -this.height/2 + 20)
            ctx.fillText(this.label, -15, 5);
          }
        });

      var labeledRect = new LabeledRect({
            width: 100,
            height: 50,
            left: 100,
            top: 100,
            label: 'test',
            fill: '#faa'
          });
          this.cbox.add(labeledRect);
    },
    createGroupElm(){
      var circle = new fabric.Circle({
        radius: 100,
        fill: '#eef',
        scaleY: 0.5,
        originX: 'center',
        originY: 'center'
      });

      var text = new fabric.Text('hello world', {
        fontSize: 30,
        originX: 'center',
        originY: 'center'
      });

      var group = new fabric.Group([ circle, text ], {
        left: 150,
        top: 100,
        angle: -10
      });

      this.cbox.add(group);
    },
    modifytoredcircle(){
      var text = new fabric.Text('hello\n world', 
        { 
          left: 150, 
          top: 150,
          fontFamily: 'Impact',
          stroke: '#c3bfbf',
          strokeWidth: 3,
          textAlign: 'right',
          fontSize: 50,
          shadow: 'rgba(0,0,0,0.3) 5px 5px 5px',
          textBackgroundColor: 'rgb(0,200,0)'
        });
      this.cbox.add(text);

      var rect1 = new fabric.Rect();
      console.log('rect1:',rect1,rect1.get('width'))
      var that = this;
      console.log('this.rect:',this.rect)
      var line = new fabric.Line(
            [280, 100, 200, 50], 
            {
              width: 5,
              fill: 'red',
              stroke: 'brown',
              strokeWidth: 5,
            }
          )
      console.log(line)
      this.cbox.add(line);

      // add images by element
      var img1 = document.getElementById('img1');
      var imgInstance = new fabric.Image(img1, {
            left: 200,
            top: 0,
            scale:2,
            angle: 0,
            opacity: 0.85
          });

      this.cbox.add(imgInstance);
      imgInstance.set({ width: 50, height: 50});

      console.log('canvas-toJSON',this.cbox.toJSON())
      console.log('canvas-toObject',this.cbox.toObject())

      // add images by url
      /*fabric.Image.fromURL('./logo.png', function(oimg){
        that.cbox.add(oimg)
      })*/


      // this.cbox.renderAll();
      // console.log('toLocalPoint:',this.rect.toLocalPoint(this.rect.Point))


/*      console.log('this.rect.toJSON:',typeof this.rect.toJSON())
      console.log('this.rect.toObject:',typeof this.rect.toObject())
*/
      /*var that = this;
      console.log('this.rect:',this.rect)
      // this.rect.setColor('#69f');
      this.rect.setGradient('fill', {
        type: 'linear',
        x1: -that.rect.width / 2,
        y1: 0,
        x2: that.rect.width / 2,
        y2: 0,
        colorStops: {
          0: 'red',
          0.5: '#005555',
          1: 'rgba(0,0,255,0.5)'
        }
      });
      this.cbox.renderAll();*/

      /*var yuan = new fabric.Circle({
        left: 0,
        top:0,
        radius: 30,
        fill:'red'
      })
      this.rect.render(yuan)*/
      
    }
  }// methods end
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
