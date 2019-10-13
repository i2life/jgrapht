# 开源库JGraphT介绍

> a Java library of graph theory data structures and algorithms.

## maven依赖
```xml
<dependency>
      <groupId>org.jgrapht</groupId>
      <artifactId>jgrapht-core</artifactId>
      <version>1.3.0</version>
    </dependency>
```

## Graph对象
Graph对象是一个泛型接口，V和E分别表示顶点类型和边类型。
对于实例化的Graph对象，通过`addVertex()`方法和`addEdge()`等方法添加顶点和边。也提供了`getEdge()`和`getVertexSupplier()`等方法来访问图里的元素。


```java
public interface Graph<V, E> {
  double DEFAULT_EDGE_WEIGHT = 1.0D;

  Set<E> getAllEdges(V var1, V var2);

  E getEdge(V var1, V var2);

  Supplier<V> getVertexSupplier();

  Supplier<E> getEdgeSupplier();

  E addEdge(V var1, V var2);

  boolean addEdge(V var1, V var2, E var3);

  V addVertex();

  boolean addVertex(V var1);

  boolean containsEdge(V var1, V var2);

  boolean containsEdge(E var1);

  boolean containsVertex(V var1);

  Set<E> edgeSet();

  int degreeOf(V var1);

  Set<E> edgesOf(V var1);

  int inDegreeOf(V var1);

  Set<E> incomingEdgesOf(V var1);

  int outDegreeOf(V var1);

  Set<E> outgoingEdgesOf(V var1);

  boolean removeAllEdges(Collection<? extends E> var1);

  Set<E> removeAllEdges(V var1, V var2);

  boolean removeAllVertices(Collection<? extends V> var1);

  E removeEdge(V var1, V var2);

  boolean removeEdge(E var1);

  boolean removeVertex(V var1);

  Set<V> vertexSet();

  V getEdgeSource(E var1);

  V getEdgeTarget(E var1);

  GraphType getType();

  double getEdgeWeight(E var1);

  void setEdgeWeight(E var1, double var2);

  default void setEdgeWeight(V sourceVertex, V targetVertex, double weight) {
    this.setEdgeWeight(this.getEdge(sourceVertex, targetVertex), weight);
  }
}

```

## 图结构类型
JGraphT支持的图结构类型如下：

![](https://raw.githubusercontent.com/i2life/imageBed/master/graph.JPG)

## 监听图的变化事件
给Graph添加监听器，可以监听到图修改事件的发生，并通过override对应的方法，监听到图修改后进行相应的处理。

```java
public interface GraphListener<V, E> extends VertexSetListener<V> {
  void edgeAdded(GraphEdgeChangeEvent<V, E> var1);

  void edgeRemoved(GraphEdgeChangeEvent<V, E> var1);

  default void edgeWeightUpdated(GraphEdgeChangeEvent<V, E> e) {
  }
}

public interface VertexSetListener<V> extends EventListener {
  void vertexAdded(GraphVertexChangeEvent<V> var1);

  void vertexRemoved(GraphVertexChangeEvent<V> var1);
}
```

## 通过拓扑模式进行图生成
实现接口`GraphGenerator<V, E, V>`预定义图生成器:

```java
public interface GraphGenerator<V, E, T> {
  void generateGraph(Graph<V, E> var1, Map<String, T> var2);

  default void generateGraph(Graph<V, E> target) {
    this.generateGraph(target, (Map)null);
  }
}
```

内置的图生成器：

![](https://raw.githubusercontent.com/i2life/imageBed/master/generator.JPG)

## 图遍历

![](https://raw.githubusercontent.com/i2life/imageBed/master/traverse.JPG)

```java
//深度优先遍历
    DepthFirstIterator iter = new DepthFirstIterator<>(teacherGraph);

    while (iter.hasNext()) {
      Teacher x = (Teacher)iter.next();

      System.out.println(x.toString());
    }
```

## 图算法

![](https://raw.githubusercontent.com/i2life/imageBed/master/algorithm.JPG)

![](https://raw.githubusercontent.com/i2life/imageBed/master/shortest.JPG)

## 图的导入导出
```java
GraphMLImporter<CustomVertex, DefaultWeightedEdge> importer =
            new GraphMLImporter<>(vertexProvider, edgeProvider);
            
GraphMLExporter<CustomVertex,
            DefaultWeightedEdge> exporter = new GraphMLExporter<>(vertexIdProvider, vertexLabelProvider, vertexAttributeProvider, edgeIdProvider,edgeLabelProvider, edgeAttributeProvider);
```

## 图的可视化
使用JGraphX适配器对图进行可视化：

```java
ListenableGraph<String, DefaultEdge> g =
            new DefaultListenableGraph<>(new DefaultDirectedGraph<>(DefaultEdge.class));

        // create a visualization using JGraph, via an adapter
        jgxAdapter = new JGraphXAdapter<>(g);
```

![](https://raw.githubusercontent.com/i2life/imageBed/master/visualization.JPG)







