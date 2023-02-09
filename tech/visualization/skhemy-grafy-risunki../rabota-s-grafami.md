# Работа с графами

Есть очень популярная спецификация — [Graphviz](https://graphviz.org/resources/).

У этой спецификации есть байдинги на множество языков (в том числе Python и Go). Под капотом они дергают бинарь `dot` (который является частью graphviz).

```
brew install graphviz
cat graph.dot | dot -Tpng -o graph.png
cat graph.dot | dot -Tsvg -o graph.svg
cat graph.dot | dot -Tjson -o graph.json

// На больших графах он у меня повис
```

А так же программы-визуализаторы под разные платформы (в тч онлайн).

Такие графы можно импортировать/экспортировать (имеют расширение `.dot`).

Пример использования в Go: [https://github.com/ofabry/go-callvis](https://github.com/ofabry/go-callvis)

## G6 Graph vizualization engine

Библиотека для визуализации графов в React, Angular, Vue, ...

Site: [https://antv-g6.gitee.io/en](https://antv-g6.gitee.io/en)
