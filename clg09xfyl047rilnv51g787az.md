---
title: "Routing in Expo: Expo Router"
seoTitle: "Routing in Expo: Expo Router"
seoDescription: "Learn how to install and configure Expo Router, create routes in your app, and consume a REST API. Get started today with expo router"
datePublished: Mon Apr 03 2023 03:30:39 GMT+0000 (Coordinated Universal Time)
cuid: clg09xfyl047rilnv51g787az
slug: routing-in-expo-expo-router
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1680483942170/f07d346f-ec17-470e-bc31-a42287a6f8c5.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1680488010036/72e32119-89ad-4a1d-9087-b097b7435fdb.png
tags: javascript, react-native, apis, expo

---

Expo is a platform that enables building native iOS and Android apps using React Native, while Expo Router is a file-based router designed for universal React Native apps that can be utilized to incorporate navigation into your application. it makes every file in our `app` directory a route. In this article, we will explore how to use Expo router by building a [Rick and Morty](https://en.wikipedia.org/wiki/Rick_and_Morty) app that consumes a [REST API](https://rickandmortyapi.com/documentation).

get code on Github: [https://github.com/evergreenx/ricknmorty-expo-router](https://github.com/evergreenx/ricknmorty-expo-router)

Here is a preview of what we will be building

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680484907224/04e8be8e-331a-49b3-958f-4cbcd5049782.gif?height=600 align="center")

## Prerequisites

Before we dive into the building, there are a few prerequisites you should be familiar with :

* Node js v14+.
    
* **Basic knowledge of react native**: Expo is built on react native, so you should have some understanding of the core concepts of React Native.
    
* **Familiarity with JavaScript**: Our application will be built using JavaScript, so you should have a basic understanding of the language.
    
* **Understanding of REST API**: you should have a basic understanding to consume a REST API.
    

If you are new to any of these concepts, I recommend taking some time to learn them before continuing with this article.

## What is Expo Router?

It is a routing concept built on top of [React Navigation suite](https://reactnavigation.org/). if you are familiar with how Nextjs handles navigation that should give you a basic understanding of Expo router. that is every file in your `app` directory becomes a route in your app.

## Let's Get Started

To get started, head over to [expo documentation](https://expo.github.io/router/docs/) to install Expo cli and Expo Go, if you already don't have it installed. Now you need to create a new expo project with Expo CLI. You can use the following command to create a new expo typescript project:

```javascript
npx create-react-native-app -t with-typescript ricknmorty-expo-router
```

Once, it is done installing our project, navigate to the project directory and start the development server by running:

```javascript
cd ricknmorty-expo-router
npx expo start
```

This will launch the Expo development server, which you can preview your app in the browser or on your phone using [Expo Go](https://docs.expo.dev/get-started/installation/#expo-go-app-for-android-and-ios).

If all went well you should have a screen preview like this

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680136259933/7b391efe-2cb1-4290-8b61-3b1483e7284e.png?height=600 align="left")

---

### Install Expo Router

Now let's install expo router and its peer dependencies :

```javascript
npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar
```

After successful installation, let's configure our app to use expo router.

Create a new file `index.ts` in the root of your project. If it exists already, replace it with the following code :

```javascript
import "expo-router/entry";
```

Also, create `app.json` file and add this code, we are adding a deep linking scheme to our app.

```javascript
{
  "expo": {
    "scheme": "myapp",

    "web": {
      "bundler": "metro"
    }
  }
}
```

Update your `package.json` with this code.

```javascript
// If you use Yarn
"resolutions": {
    "metro": "0.76.0",
    "metro-resolver": "0.76.0"
  }

// if you use npm 
"overrides": {
    "metro": "0.76.0",
    "metro-resolver": "0.76.0"
  }
```

Next, we have to update our `babel.config.js` file and replace the content with the code below.

```javascript
module.exports = function (api) {
  api.cache(true);
  return {
    presets: ["babel-preset-expo"],
    plugins: [require.resolve("expo-router/babel")],
  };
};
```

Let us create a new directory called `app` , this is where our routes will live, go ahead and add a new `index.tsx` file in your just-created directory. your project should look something like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680191019435/c4cdf755-ea35-4798-a2dc-fef23b789ed7.png align="left")

After this add a react function component to `index.tsx` and delete `App.tsx`

```javascript
import { View, Image, ViewStyle } from "react-native";
import React from "react";

const Index = () => {
  return (
    <View style={$ViewStyle}>
    </View>
  );
};

export default Index;

const $ViewStyle: ViewStyle = {
  flex: 1,
  padding: 20,
  backgroundColor: "#FFDEAD",
};
```

Now restart your server.

```javascript
npx expo start
```

---

Here is a preview of how our app looks

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680316021045/f74533a0-ca51-4e6c-8589-b25b0657381b.jpeg?height=600 align="center")

### Fetch Data

We have successfully set up our project with Expo Router and `index.js` now serves as our home screen. Next, we are going to consume the [rick and morty](https://rickandmortyapi.com/documentation) API. For this article, we will be using only the character's resources. To consume the API we are going to make use of [**Apisauce**](https://github.com/infinitered/apisauce) which is an HTTP client wrapper built on Axios. Let's install the package.

```javascript
// npm
npm i apisauce

// yarn
yarn add apisauce
```

Create a `interface.ts` file in the root of our application, this is where the [interface](https://www.typescriptlang.org/docs/handbook/interfaces.html) for our Characters API response will live.

```javascript
// interface.ts

  export interface Location {
    name: string;
    url: string;
  }
  
export interface Character {
    id: number;
    name: string;
    status: string;
    species: string;
    type: string;
    gender: string;
    origin: Location;
    location: Location;
    image: string;
    medium : string;
    episode: string[];
    url: string;
    created: string;
  }

  export interface ApiResponse {
    info: {
      count: number;
      pages: number;
      next: string;
      prev: string;
    };
    results: Character[];
  }
```

Moving forward create a new file called `api.ts` and copy this code.

```javascript
import { create } from "apisauce";
import { ApiResponse} from "./interface";
const api = create({
  baseURL: "https://rickandmortyapi.com/api/",
  headers: { Accept: "application/json" },
});

export const getCharacters = async (): Promise<ApiResponse> => {
  const response = await api.get("/character");
  return response.data as ApiResponse;
};
```

Firstly we are importing `create` from apisauce and also importing the interface we created earlier. Next, we are defining the API with the `create` method. The `getCharacters` function is an async function that makes a GET request to the `/characters` endpoint. Open and replace the `index.tsx` component with the code below.

```javascript
import { View, Image, ViewStyle, Text, TextStyle } from "react-native";
import React, { useEffect, useState } from "react";
import { getCharacters } from "../api";
import { Character } from "../interface";
import CharacterCard from "../component/CharacterCard";

const Index = () => {
  const [characters, setCharacters] = useState<Character[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string>("");

  useEffect(() => {
    setLoading(true);
    const fetchCharactersData = async () => {
      try {
        const res = await getCharacters();
        setCharacters(res.results);
      } catch (err) {
        setLoading(false);
        setCharacters([]);
        setError("An error occurred");
      } finally {
        setLoading(false);
      }
    };
    fetchCharactersData();
  }, []);
  console.log(characters);

  return (
    <View style={$container}>
      <View>
        <Image
          source={{
            uri: "https://res.cloudinary.com/evergreenx/image/upload/v1680259111/Logo_1_tdttqu.png",
          }}
          resizeMode="contain"
          alt="logo"
          style={{ width: 300, height: 200, alignSelf: "center" }}
        />
      </View>

      {loading && (
        <Image
          source={{
            uri: "https://res.cloudinary.com/evergreenx/image/upload/v1680297056/Loading_component_tr2ec6.png",
          }}
          resizeMode="contain"
          style={{ width: 300, height: 200, alignSelf: "center" }}
        />
      )}
      {error && <Text style={$errorText}>{error}</Text>}
      {characters && <CharacterCard characters={characters} />}
    </View>
  );
};

export default Index;

const $container: ViewStyle = {
  flex: 1,
  width: "100%",
  backgroundColor: "#FFDEAD",
};

const $errorText: TextStyle = {
  color: "red",
  fontSize: 20,
  alignSelf: "center",
};
```

We imported our async function and used a useEffect to get our data on-load of the component, The error and data have been saved in a useState. We are also importing `CharacterCard` component which does not exist yet ,it will be used to display the data.

---

### Show Characters

Now let's show the data, create a component folder at the root of the project, and add a new file called `CharacterCard.tsx` .

```javascript
import {
  View,
  Text,
  FlatList,
  TextStyle,
  ViewStyle,
  Image,
} from "react-native";
import React from "react";
import { Character } from "../interface";
type CharacterCardProps = {
  characters: Character[];
};

export default function characterCard({ characters }: CharacterCardProps) {
  const renderCharacters = ({ item }: { item: Character }) => {
    return (
      <View style={$characterContainer}>
        <View style={$characterImageContainer}>
          <Image
            source={{ uri: item?.image }}
            style={{
              width: "100%",
              height: 150,
              borderBottomLeftRadius: 0,
              borderBottomRightRadius: 0,
              borderRadius: 10,
            }}
            resizeMode="cover"
          />
        </View>

        <View style={$characterInfoContainer}>
          <Text style={$characterText}>{item?.name}</Text>
          <Text style={$characterSpecies}>{item?.species}</Text>
        </View>
      </View>
    );
  };

const renderHeader = () => {
    return (
      <View>
        <Image
          source={{
            uri: "https://res.cloudinary.com/evergreenx/image/upload/v1680259111/Logo_1_tdttqu.png",
          }}
          style={{ width: 300, height: 200, alignSelf: "center" }}
          resizeMode="contain"
          alt="logo"
        />
      </View>
    );
  };


  return (
    <View style={$container}>
      <FlatList
        style={{ width: "100%", padding: 10 }}
        data={characters}
        renderItem={renderCharacters}
        keyExtractor={(item) => item.id.toString()}
        showsHorizontalScrollIndicator={true}
        numColumns={2}
        ListHeaderComponent={renderHeader}
      />
    </View>
  );
}

const $container: ViewStyle = {
  flex: 1,
  alignItems: "center",
  justifyContent: "center",
};

const $characterContainer: ViewStyle = {
  backgroundColor: "#fff",
  height: 230,
  width: "45%",
  borderRadius: 10,
  marginVertical: 10,
  marginHorizontal: 10,
};
const $characterImageContainer: ViewStyle = {
  width: "100%",
  overflow: "hidden",
  marginBottom: 10,
};

const $characterText: TextStyle = {
  color: "#000",
  fontSize: 18,
  fontWeight: "500",
};

const $characterSpecies: TextStyle = {
  color: "#000",
  fontSize: 13,
  fontWeight: "500",
  marginTop: 5,
};

const $characterInfoContainer: ViewStyle = {
  padding: 10,
};
```

Let's break down what the code does, so we render a grid of cards containing each character, also the component takes a prop `characters` which is an array of objects. The component uses a `FlatList` to render the cards and the `keyExtractor` prop is used to extract a unique key for each item in the array, `numcolumns` prop is set to display the cards in a grid with two columns and the `showsVerticalScrollIndicator` prop is set to `false` to hide the vertical scroll indicator. `renderHeader` prop is used to render a component on top of the list, we render an image component with our image source hosted on [Cloudinary](https://cloudinary.com/).

We define `renderCharacters` a function that takes an object with an item property that represents a single character from the `characters` array. The function returns a JSX representation of a character card and lastly, and we define some styles to make the cards look good.

So far our app is coming together piece by piece. it should look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680310588664/cb934451-3277-49a8-87e2-db579e6ce0de.jpeg?height=600 align="center")

### Move Between Screens

One should be able to move between screens and we will see how to do that in Expo Router. To achieve this expo router has a [`<Link`](https://expo.github.io/router/docs/features/linking)`/>` component or `useRouter` hook. so let's make it when we click on a character we move to a dynamic route to view that particular character's details.

Head over to the project and create a new folder inside the `app` directory and name it `details` , then create a file `[id].tsx` inside this folder. This will serve as our dynamic route screen, where characters' details will be shown. Expo router makes it easy to add navigation on the fly. Add the code to our just created screen.

```javascript
import { View, Text, ViewStyle, TextStyle, Image } from "react-native";
import React, { useEffect, useState } from "react";
import { Link, Stack } from "expo-router";
import { useSearchParams } from "expo-router";
import { getCharacter } from "../../api";

const Details = () => {
  const [character, setCharacter] = useState<any>(null);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string>("");
  const { id } = useSearchParams();

  useEffect(() => {
    const fetchCharactersData = async () => {
      try {
        const res = await getCharacter(id);
        setCharacter(res);
      } catch (err) {
        setLoading(false);
        setCharacter([]);
        setError("An error occurred");
      } finally {
        setLoading(false);
      }
    };
    fetchCharactersData();
  }, []);

  return (
    <View style={$container}>
      {error && <Text style={$errorText}>{error}</Text>}

      {loading && (
        <Image
          source={{
            uri: "https://res.cloudinary.com/evergreenx/image/upload/v1680297056/Loading_component_tr2ec6.png",
          }}
          resizeMode="contain"
          style={{
            width: 300,
            height: 200,
            alignSelf: "center",
            marginTop: 100,
          }}
        />
      )}
      {character && (
        <>
          <Link
            href={{
              pathname: "/",
            }}
            style={$backButton}
          >
            <Text style={$backButtonText}>Back</Text>
          </Link>

          <View style={$characterImageContainer}>
            <Image
              source={{ uri: character?.image }}
              style={{
                width: 250,
                height: 250,
                borderRadius: 150,
              }}
              resizeMode="contain"
            />
            <Text style={$characterName}>{character?.name}</Text>
          </View>

          <View style={$characterInfoContainer}>
            <View style={$characterInfo}>
              <Text style={$characterText}>Gender: </Text>
              <Text style={$characterTextInfo}>{character?.gender}</Text>
            </View>

            <View style={$characterInfo}>
              <Text style={$characterText}>Status: </Text>
              <Text style={$characterTextInfo}>{character?.status}</Text>
            </View>

            <View style={$characterInfo}>
              <Text style={$characterText}>Species: </Text>
              <Text style={$characterTextInfo}>{character?.species}</Text>
            </View>

            <View style={$characterInfo}>
              <Text style={$characterText}>origin: </Text>
              <Text style={$characterTextInfo}>{character?.origin.name}</Text>
            </View>

            <View style={$characterInfo}>
              <Text style={$characterText}>location: </Text>
              <Text style={$characterTextInfo}>{character?.location.name}</Text>
            </View>
          </View>
        </>
      )}
    </View>
  );
};

export default Details;

const $container: ViewStyle = {
  flex: 1,
  width: "100%",
  backgroundColor: "#FFDEAD",

  paddingVertical: 40,
  paddingHorizontal: 20,
};

const $characterImageContainer: ViewStyle = {
  width: "100%",
  padding: 15,
  borderRadius: 10,
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
};

const $characterName: TextStyle = {
  color: "#081F32",
  marginVertical: 16,
  fontSize: 30,
  fontWeight: "400",
};

const $characterInfoContainer: ViewStyle = {
  width: "100%",
  padding: 15,
  borderRadius: 10,
};

const $characterText: TextStyle = {
  color: "#081F32",
  marginVertical: 3,
  fontSize: 16,
  fontWeight: "700",
  textTransform: "capitalize",
  display: "flex",
  flexDirection: "row",
};

const $characterTextInfo: TextStyle = {
  color: "#6E798C",
  fontSize: 14,
  fontWeight: "400",
};

const $characterInfo: ViewStyle = {
  padding: 5,
  marginVertical: 3,
  borderBottomColor: "#979797",
  borderBottomWidth: 0.5,
};

const $backButton: ViewStyle = {
  width: 100,
  height: 40,
  borderRadius: 10,
  display: "flex",
  justifyContent: "center",
  alignItems: "center",
  marginBottom: 20,
  marginTop: 10,
};

const $backButtonText: TextStyle = {
  color: "#979797",
  fontSize: 16,
  fontWeight: "700",
};

const $errorText: TextStyle = {
  color: "red",
  fontSize: 16,
  fontWeight: "700",
  alignSelf: "center",
  paddingTop: 100,
};
```

In the code above we import Link and useSearchParams for expo router. We will be using Link to a back button and useSearchParams to return the URL search parameters. we imported the async function `getCharacter` which is yet to be created.

A useEffect to fetch our data for a single character by the `id` . Check API documentation here. We are setting some states for our data. lastly, we are returning JSX based on our data. for the `Link` component from expo router, it takes a `href` prop. and this takes an object with a pathname that `/` represents our home screen. we could also define this way `<Link href="/" >`

Now let's update our `api` file with the `getCharacter` function. replace your code with this below.

```javascript
import { create } from "apisauce";
import { ApiResponse, Character } from "./interface";
const api = create({
  baseURL: "https://rickandmortyapi.com/api/",
  headers: { Accept: "application/json" },
});

export const getCharacters = async (): Promise<ApiResponse> => {
  const response = await api.get("/character");
  if (!response.ok) throw new Error(response.problem);
  return response.data as ApiResponse;
};

export const getCharacter = async (id: string): Promise<Character> => {
  const response = await api.get(`/character/${id}`);
  if (!response.ok) throw new Error(response.problem);
  return response.data as Character;
};
```

We added a new async function `getCharacter` to fetch a single character based on the `id` .

Let update `CharacterCard.tsx` so when we click on a card we will be taken to a new screen with details containing that particular character.

Update the component with the code below.

```javascript
import {
  View,
  Text,
  FlatList,
  TextStyle,
  ViewStyle,
  Image,
  Pressable,
} from "react-native";
import React from "react";
import { Character } from "../interface";


import { useRouter } from "expo-router";
type CharacterCardProps = {
  characters: Character[];
};

export default function characterCard({ characters }: CharacterCardProps) {
  const router = useRouter();
  const renderCharacters = ({ item }: { item: Character }) => {
    return (
      <Pressable
        style={$characterContainer}
        onPress={() => {
          router.push({
            pathname: "details/id/",
            params: { id: item?.id },
          });
        }}
      >
        <View style={$characterImageContainer}>
          <Image
            source={{ uri: item?.image }}
            style={{
              width: "100%",
              height: 150,
              borderBottomLeftRadius: 0,
              borderBottomRightRadius: 0,
              borderRadius: 10,
            }}
          />
        </View>

        <View style={$characterInfoContainer}>
          <Text style={$characterText}>{item?.name}</Text>
          <Text style={$characterSpecies}>{item?.species}</Text>
        </View>
      </Pressable>
    );
  };

  const renderHeader = () => {
    return (
      <View>
        <Image
          source={{
            uri: "https://res.cloudinary.com/evergreenx/image/upload/v1680259111/Logo_1_tdttqu.png",
          }}
          style={{ width: 300, height: 200, alignSelf: "center" }}
          resizeMode="contain"
          alt="logo"
        />
      </View>
    );
  };

  return (
    <View style={$container}>
      <FlatList
        style={{ width: "100%", padding: 10, marginBottom: 20 }}
        data={characters}
        renderItem={renderCharacters}
        keyExtractor={(item) => item.id.toString()}
        showsVerticalScrollIndicator={false}
        numColumns={2}
        ListHeaderComponent={renderHeader}
      />
    </View>
  );
}

const $container: ViewStyle = {
  flex: 1,
  alignItems: "center",
  justifyContent: "center",
};

const $characterContainer: ViewStyle = {
  backgroundColor: "#fff",
  height: 230,
  width: "45%",
  borderRadius: 10,
  marginVertical: 10,
  marginHorizontal: 10,
};
const $characterImageContainer: ViewStyle = {
  width: "100%",
  overflow: "hidden",
  marginBottom: 10,
};

const $characterText: TextStyle = {
  color: "#000",
  fontSize: 18,
  fontWeight: "500",
};

const $characterSpecies: TextStyle = {
  color: "#000",
  fontSize: 13,
  fontWeight: "500",
  marginTop: 5,
};

const $characterInfoContainer: ViewStyle = {
  padding: 10,
};
```

For the new changes contained in the code above, we imported `useRouter` hook from expo router, and we will use it to navigate to the dynamic screen we created earlier. As stated in the docs we could use the Link component to move between and we can also use the useRouter hook to navigate imperatively.

On our card, we added a `Pressable` component, onPress of the card we will be taken to the `details/id` and we are passing a param to the screen which is `id` representing a single character. Then use the `id` to get data from our API for that particular character.

When we click on a card, we should get a screen like this containing details of the character you clicked on.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1680457969329/19386b8e-4d48-4496-9138-af9a23418ebe.jpeg?height=600 align="center")

## Conclusion

In the article, we have explored how to use the Expo Router to add navigation to our app and also how we could fetch data from an API using apisauce.

This is a little of what we could do with Expo Router, check out the docs to learn more.

* [https://expo.github.io/router/docs/guides/](https://expo.github.io/router/docs/guides/)
    
* [https://expo.github.io/router/docs/guides/tabs](https://expo.github.io/router/docs/guides/tabs)
    
* [https://expo.github.io/router/docs/guides/headers](https://expo.github.io/router/docs/guides/headers)
    
* [https://expo.github.io/router/docs/guides/auth](https://expo.github.io/router/docs/guides/auth)
    

## **Credits**

* [https://expo.github.io/router/docs](https://expo.github.io/router/docs)
    
* [https://docs.expo.dev/](https://docs.expo.dev/)
    
* [https://github.com/infinitered/apisauce](https://github.com/infinitered/apisauce)
    
* [https://rickandmortyapi.com/documentation/](https://rickandmortyapi.com/documentation/)