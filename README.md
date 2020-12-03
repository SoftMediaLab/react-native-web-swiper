# react-native-web-swiper

Simple swiper / slider. Works both on React-Native and React-Native-Web.

## Demo

Hybrid Snack: https://snack.expo.io/@oxyii/react-native-web-swiper

## Installation

```bash
$ npm i react-native-web-swiper --save
```

## Usage

```jsx
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import Swiper from 'react-native-web-swiper';

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  slideContainer: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
  },
  slide1: {
    backgroundColor: 'rgba(20,20,200,0.3)',
  },
  slide2: {
    backgroundColor: 'rgba(20,200,20,0.3)',
  },
  slide3: {
    backgroundColor: 'rgba(200,20,20,0.3)',
  },
});

export default class Screen extends React.Component {
  render() {
    return (
      <View style={styles.container}>
        <Swiper>
          <View style={[styles.slideContainer,styles.slide1]}>
            <Text>Slide 1</Text>
          </View>
          <View style={[styles.slideContainer,styles.slide2]}>
            <Text>Slide 2</Text>
          </View>
          <View style={[styles.slideContainer,styles.slide3]}>
            <Text>Slide 3</Text>
          </View>
        </Swiper>
      </View>
    );
  }
}
```

### With props

```jsx
<Swiper
  vertical {/* slide up / down instead left / right */}
  from={1} {/* initial slide is second */}
  loop {/* back to first slide after last */}
  timeout={2} {/* autoplay duration (2 secs) */}
  springConfig={{ speed: 11 }} {/* RN Animated.spring config */}
  minDistanceForAction={0.15} {/* Swipe less that 15% keep active slide */}
  positionFixed {/* Fix mobile safari vertical bounces */}
  controlsProps={{
    DotComponent: ({ index, isActive, onPress }) => <Text onPress={onPress}>Your Custom Dot {index+1}</Text>
  }}
>
  <View style={{ flex: 1 }} /> {/* Slide 1 */}
  <View style={{ flex: 1 }} /> {/* Slide 2 */}
  {/* ... */}
</Swiper>
```

### Dynamic content

The Swiper will not be re-rendered if slides state or props have changed. Slides must control their condition.

```jsx
import React from 'react';
import { Text, View } from 'react-native';

export class SomeDynamicSlide extends React.Component {
  state = {
    someStateItem: 'someValue',
  };

  render() {
    return (
      <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
        <Text onPress={() => this.setState({ someStateItem: 'newValue' })}>
            {this.state.someStateItem}
        </Text>
      </View>
    );
  }
}

...

import React from 'react';
import { Text, View } from 'react-native';
import Swiper from 'react-native-web-swiper';
import { SomeDynamicSlide } from 'someDynamicSlideFile';

class SwiperWrapper extends React.Component {
  render() {
    return (
      <Swiper>

        <SomeDynamicSlide />

        <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
          <Text>This is static content slide</Text>
        </View>

      </Swiper>
    );
  }
}

``` 

---

## Props

|         Prop         |    Default   |          Type         | Description |
| :------------------- |:------------:| :--------------------:| :-----------|
| vertical             | `false`      | `boolean`             | Swiper vertical layout |
| from                 | `0`          | `number`              | Initial slide index |
| loop                 | `false`      | `boolean`             | Set to `true` to enable continuous loop mode |
| loopAnimation        | `spring`     | `spring` \| `extension` | Loop animation variant |
| timeout              | `0`          | `number`              | Delay between auto play transitions (in second). Set negative value for reverse autoplay :satisfied:. Autoplay disabled by default |
| gesturesEnabled      | `true`       | `boolean`             | Function that returns boolean value. Set to `false` to disable swiping mechanism. Allow to use Prev / Next buttons only |
| springConfig         |              | [`Animated.spring`](https://facebook.github.io/react-native/docs/animated#spring) | Tune spring animation on autoplay, touch release or slides changes via buttons |
| minDistanceToCapture | `5`          | `number`              | Initiate animation after swipe this distance. It fix gesture collisions inside ScrollView |
| minDistanceForAction | `0.2`        | `number`              | Minimal part of swiper width (or height for vertical) must be swiped for changing index. Otherwise animation restore current slide. Default value 0.2 means that 20% must be swiped for change index |
| positionFixed        | `false`      | `boolean`             | Swiper inner container position `fixed` instead `relative`. Fix mobile safari vertical bounce |
| containerStyle       |              | `ViewPropTypes.style` | Outer (root) container style |
| innerContainerStyle  |              | `ViewPropTypes.style` | Inner container style |
| swipeAreaStyle       |              | `ViewPropTypes.style` | Swipe area style |
| slideWrapperStyle    |              | `ViewPropTypes.style` | Each slide wrapper style |
| slidesPerView        | `1`          | `number`              | Number of slides per view (slides visible at the same time on slider's container). |
| controlsEnabled      | `true`       | `boolean`             | Dots and control buttons visible and enabled |
| Controls             |              | `React.Component`     | Custom controls component |
| onAnimationStart     |              | `function`            | Any swiper animation start |
| onAnimationEnd       |              | `function`            | Any swiper animation end |
| onIndexChanged       |              | `function`            | Called when active index changed |
| controlsProps        |              | `object`              | see below |
| autoHeight           | `false`      | `boolean`             | Calculate height from children elements |

### Controls Props

Over the swiper we need to create a controls layer. But this layer will block the possibility of swiper layer control.
We created 9 controls placeholders to solve this problem:
`top-left`, `top`, `top-right`, `left`, `center`, `right`, `bottom-left`, `bottom` and `bottom-right`.
You can adjust controls position by placing into relevant placeholder:

```jsx
<Swiper
  ...
  controlsProps={{
    prevTitle: 'prev button title',
    nextTitle: 'next button title',
    dotsTouchable: true, {/* touch over dot will make swiper move to rel slide */}
    dotsPos: 'top',
    prevPos: false, {/* hide prev button */}
    nextPos: 'top-right',
    cellsStyle: {
      'top': { padding: 5, backgroundColor: 'rgba(0, 0, 0, .3)' },
      'top-left': { /* any custom placeholder style */ },
    },
    cellsContent: {
      'bottom-right': <AnyComponent /> {/* Additional content in placeholder */}
    }
  }}
/>
```

|         Prop         |    Default   |           Type           | Description |
| :------------------- |:------------:| :-----------------------:| :-----------|
| cellsStyle           |              | `object`                 | Controls corners placeholders styles. Allowed keys is: `top-left`, `top`, `top-right`, `left`, `center`, `right`, `bottom-left`, `bottom` and `bottom-right`, allowed values is `ViewPropTypes.style` |
| cellsContent         |              | `object`                 | Controls corners placeholders additional content. Allowed keys is: `top-left`, `top`, `top-right`, `left`, `center`, `right`, `bottom-left`, `bottom` and `bottom-right`, allowed values is `string` **OR** `React element` |
| dotsPos              | `'bottom'` **OR** `'right'` if vertical | `boolean` **OR** `enum('top-left', 'top', 'top-right', 'left', 'center', 'right', 'bottom-left', 'bottom', 'bottom-right')` | Dots position |
| prevPos              | `'bottom-left'` **OR** `'top-right'` if vertical | `boolean` **OR** `enum('top-left', 'top', 'top-right', 'left', 'center', 'right', 'bottom-left', 'bottom', 'bottom-right')` | Prev button position |
| nextPos              | `'bottom-right'` | `boolean` **OR** `enum('top-left', 'top', 'top-right', 'left', 'center', 'right', 'bottom-left', 'bottom', 'bottom-right')` | Next button position |
| prevTitle            | `'Prev'`     | `string`                 | Prev button title |
| nextTitle            | `'Next'`     | `string`                 | Next button title |
| prevTitleStyle       |              | `Text.propTypes.style`   | Customize prev button title |
| nextTitleStyle       |              | `Text.propTypes.style`   | Customize next button title |
| PrevComponent        |              | `React.Component`        | Custom prev button component |
| NextComponent        |              | `React.Component`        | Custom next button component |
| firstPrevElement     |              | `element`                | Custom prev element on first slide (if not loop) |
| lastNextElement      |              | `element`                | Custom next element on last slide (if not loop) |
| dotsTouchable        | `false`      | `boolean`                | Touches over dots will move swiper to relative slide |
| dotsWrapperStyle     |              | `ViewPropTypes.style`    | Dots wrapper View style |
| dotProps             |              | `object`                 | `react-native-elements` [Badge props](https://react-native-training.github.io/react-native-elements/docs/badge.html#props) |
| dotActiveStyle       |              | `object`                 | Additional style to active dot. Will be added to dot [badgeStyle](https://react-native-training.github.io/react-native-elements/docs/badge.html#badgestyle) |
| DotComponent         |              | `React.Component`        | Custom dot component |
| topRow               | `true`       | `boolean`                | Render top row       |
| centerRow            | `true`       | `boolean`                | Render center row    |
| bottomRow            | `true`       | `boolean`                | Render bottom row    |

## Interaction methods

Store a reference to the Swiper in your component by using the ref prop
provided by React ([see docs](https://reactjs.org/docs/refs-and-the-dom.html)):

```jsx
const swiperRef = useRef(null);

...

<Swiper
  ref={swiperRef}
  ...
/>
```

Then you can manually trigger swiper from anywhere:

```jsx
() => {
  swiperRef.current.goTo(1);
  swiperRef.current.goToPrev();
  swiperRef.current.goToNext();
  const index = swiperRef.current.getActiveIndex();
};
```
