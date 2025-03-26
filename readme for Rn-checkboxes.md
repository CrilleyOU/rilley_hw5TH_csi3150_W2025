# React Native Checkbox Component Tutorial

## Lesson Outcome

- Implementing Checkbox in React Native

## Revision Topics

- Setting up React Native Project using Expo
- React native core components like View, Text, StyleSheet
- React native navigation
- Redux

### Pre-requisites

- This project will be developed using Expo. So make sure you have Expo CLI installed.
- This project will use the YARN tool to manage project dependencies. So make sure you have Yarn installed before creating expo app.

```
npm install yarn
```

- If you want to test your App development on an android emulator or iOS simulator, make sure you have android studio or xCode installed on your system, respectively.

### Tutorial Steps

- Note: Checkbox component is not (yet) supported by the react native core library.

- Background: Previously, developers used to creatively implement checkboxes in react native by the use of other core components like TouchableOpacity, Text, and styling property of border (to give the look and feel of a checkbox) along with conditional rendering.

- Later on, the react-native-elements package (https://reactnativeelements.com/docs) gave some support for UI development of checkboxes. However, it is not actively maintained at the moment. On a side note, be mindful of this package, it is a very powerful package provided pre-styled component for various UI components and icons.

- We will be using the following package for Checkbox component implementation: https://github.com/react-native-checkbox/react-native-checkbox

#### Step 1: Project directory setup

1. Create expo app in VSCode (make sure yarn is installed as outlined earlier before you do this)

```
npx create-expo-app CheckboxTutorial
```

2. run expo app on your smartphone or android emulator or iOS simulator to make sure the react native Hello World code is running.

```
expo start
```

3. If all looks good, stop the server and install the following package. Note: you necessary do not need to stop the server before installing a package, but from experience, sometimes the server crashes due to conflict of installing a package dependency. Whereas other times a server restart is required to correctly incorporate certain other packages. To avoid these hassels, I tend to stop the server and install the packages and restart the server.

4. Add the checkbox package:

```
yarn add @react-native-community/checkbox
```

For iOS you will have to add another package:

```
npx pod-install
```

5. Familiarize yourself with the contents of the documentation: https://github.com/react-native-checkbox/react-native-checkbox

Especially the usage and props sections.

6. App logic:

   - App will first present the users to select different choices from a few categories.
   - After user does the selection, they will click a Submit button
   - In a new screen, App will show the users the list of options they have selected.

7. In the root directory of the project folder, create two sub-folders called components and screens.

8. In ./screens create two files named QuestionScreen.js and AnswerScreen.js. Populate these files with boilerplate code (if you have react native code snippet extension installed in VSCode you can simply type rnfes and hit enter. It stands for react native functional component export with styling)

9. In ./component folder create two files called QuestionCard.js and AnswerCard.js. Populate the files with boilerplate code.

10. In ./screens/QuestionScreen.js make a functional component call to QuestionCard.js. Do the same but for AnswerScreen.js screen calling AnswerCard.js

#### Step 2: react navigation setup

You can refer to my documentation for the uber-app-clone exercise on my Github repo from Step 9 bullet 10 for detailed explanation.

1. install required packages

```
yarn add @react-navigation/native
```

```
npx expo install react-native-screens react-native-safe-area-context
```

```
yarn add @react-navigation/native-stack
```

2. Open ./App.js and replace the existing code with the following code (You can check the changes made compared to the boilerplate code by yourself)

```javascript
import { StyleSheet, Text, View } from "react-native";

import { NavigationContainer } from "@react-navigation/native";
import { createNativeStackNavigator } from "@react-navigation/native-stack";
import QuestionScreen from "./screens/QuestionScreen";
import AnswerScreen from "./screens/AnswerScreen";

export default function App() {
  const Stack = createNativeStackNavigator();
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen
          name="QuestionScreen"
          component={QuestionScreen}
          options={{ headerShown: false }}
        />
        <Stack.Screen
          name="AnswerScreen"
          component={AnswerScreen}
          options={{ headerShown: false }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

3. Run expo server. If Everything goes well you should see the default text of QuestionScreen.js and QuestionCard.js on the emulator in the far top left corner. We now have to edit the QuestionScreen and component to implement checkboxes and setup navigation to the Answers screen.

#### Step 3: Implement Redux for state management.

Note: The reason why we need Redux is because, users will give their input checkboxes in the QuestionCard.js component and the App needs to take this state and pass it to the AnswerCard.js component. These components do not have parent-child hierarchy. So to maintain state using the useState hook of React library will become tedious. Hence, the choice of Redux.

For detailed instructions on how to setup Redux refer to my documentation for the uber-app-clone exercise on my Github repo from Step 2 to end of Step 4 sections.

1. install required packages

```
yarn add @reduxjs/toolkit
```

```
yarn add react-redux
```

2. Create file in root directory called ./store.js and add the following code:

```javascript
import { configureStore } from "@reduxjs/toolkit";

import quesReducer from "./slices/questionSlice";

const store = configureStore({
  reducer: {
    questionData: questionReducer,
  },
});

export default store;
```

3. Create a sub-folder in the root directory called slices, and inside it create a file called questionSlice.js, and add the following code:

```javascript
import { createSlice } from "@reduxjs/toolkit";

//initial state - all the checkbox options that we give to the user. One state for each option
/**
 * Q1: What is/are your preferred programming languages
 *   a. Python b. Java c. JavaScript d. C/C++
 *
 * Q2. Where would you apply for a job?
 *   a. Facebook b. Apple c. Netflix d. Google
 */
const initialState = {
  languageOptionPython: false,
  languageOptionJava: false,
  languageOptionJavascript: false,
  languageOptionC: false,
  companyOptionFacebook: false,
  companyOptionApple: false,
  companyOptionNetflix: false,
  companyOptionGoogle: false,
};

// create data slice
export const questionSlice = createSlice({
  name: "questionData",
  initialState,
  reducers: {
    setLanguageOptionPython: (state, action) => {
      state.languageOptionPython = action.payload;
    },
    setLanguageOptionJava: (state, action) => {
      state.languageOptionJava = action.payload;
    },
    setLanguageOptionJavascript: (state, action) => {
      state.languageOptionJavascript = action.payload;
    },
    setLanguageOptionC: (state, action) => {
      state.languageOptionC = action.payload;
    },
    setCompanyOptionFacebook: (state, action) => {
      state.companyOptionFacebook = action.payload;
    },
    setCompanyOptionApple: (state, action) => {
      state.companyOptionApple = action.payload;
    },
    setCompanyOptionNetflix: (state, action) => {
      state.companyOptionNetflix = action.payload;
    },
    setCompanyOptionGoogle: (state, action) => {
      state.companyOptionGoogle = action.payload;
    },
  },
});

// export reducers
export const {
  setLanguageOptionPython,
  setLanguageOptionJava,
  setLanguageOptionJavascript,
  setLanguageOptionC,
  setCompanyOptionFacebook,
  setCompanyOptionApple,
  setCompanyOptionNetflix,
  setCompanyOptionGoogle,
} = questionSlice.actions;

// create and export selectors
export const selectLanguageOptionPython = (state) =>
  state.questionData.languageOptionPython;

export const selectLanguageOptionJava = (state) =>
  state.questionData.languageOptionJava;

export const selectLanguageOptionJavascript = (state) =>
  state.questionData.languageOptionJavascript;

export const selectLanguageOptionC = (state) =>
  state.questionData.languageOptionC;

export const selectCompanyOptionFacebook = (state) =>
  state.questionData.companyOptionFacebook;

export const selectCompanyOptionApple = (state) =>
  state.questionData.companyOptionApple;

export const selectCompanyOptionNetflix = (state) =>
  state.questionData.companyOptionNetflix;

export const selectCompanyOptionGoogle = (state) =>
  state.questionData.companyOptionGoogle;

export default questionSlice.reducer;
```

4. In App.js wrap your NavigationContainer component in the Provider component after importing it and add the prop store and connect it with your data store file after importing it to App.js

```javascript
//import this
import store from "./store";

// add this inside return()
<Provider store={store}>// rest of the app</Provider>;
```

#### Step4: Add Checkbox component

1. Open file ./components/QuestionCard.js

BUG NOTE: The previous package discussed for Checkboxes is not compatible with Expo.

2. Use the following package for checkboxes: https://docs.expo.dev/versions/latest/sdk/checkbox/

install it by:

```
npx expo install expo-checkbox
```

3. remove the boilerplate code and add the following code:

```javascript
import { Button, SafeAreaView, StyleSheet, Text, View } from "react-native";
import React from "react";

//import for Checkbox component. Make sure package is installed~
// DOES NOT WORK ~
// import CheckBox from "@react-native-community/checkbox";

// different checkbox package
import Checkbox from "expo-checkbox";

//redux hook import to call dispatch to execute the reducers
import { useDispatch } from "react-redux";

// import all the reducers from data slice
import {
  setLanguageOptionPython,
  setLanguageOptionJava,
  setLanguageOptionJavascript,
  setLanguageOptionC,
  setCompanyOptionFacebook,
  setCompanyOptionApple,
  setCompanyOptionNetflix,
  setCompanyOptionGoogle,
} from "../slices/questionSlice";

//imports for selectors
import { useSelector } from "react-redux";
import {
  selectLanguageOptionPython,
  selectLanguageOptionJava,
  selectLanguageOptionJavascript,
  selectLanguageOptionC,
  selectCompanyOptionFacebook,
  selectCompanyOptionApple,
  selectCompanyOptionNetflix,
  selectCompanyOptionGoogle,
} from "../slices/questionSlice";

//import for navigation
import { useNavigation } from "@react-navigation/native";

const QuestionCard = () => {
  const dispatch = useDispatch();
  const languageOptionPython = useSelector(selectLanguageOptionPython);
  const languageOptionJava = useSelector(selectLanguageOptionJava);
  const languageOptionJavascript = useSelector(selectLanguageOptionJavascript);
  const languageOptionC = useSelector(selectLanguageOptionC);
  const companyOptionFacebook = useSelector(selectCompanyOptionFacebook);
  const companyOptionApple = useSelector(selectCompanyOptionApple);
  const companyOptionNetflix = useSelector(selectCompanyOptionNetflix);
  const companyOptionGoogle = useSelector(selectCompanyOptionGoogle);

  const navigation = useNavigation();
  return (
    <SafeAreaView>
      <View style={styles.questionWrapper}>
        <Text style={{ fontSize: 22, paddingBottom: 15 }}>
          Welcome, Jon Snow!
        </Text>

        <View>
          <Text style={styles.questionText}>
            Choose preferred programming languages:
          </Text>

          <View style={styles.questionOptions}>
            <Checkbox
              disabled={false}
              value={languageOptionPython}
              onValueChange={(newValue) => {
                dispatch(setLanguageOptionPython(newValue));
              }}
            />
            <Text>Python</Text>

            <View style={styles.questionOptions}>
              <Checkbox
                disabled={false}
                value={languageOptionJava}
                onValueChange={(newValue) => {
                  dispatch(setLanguageOptionJava(newValue));
                }}
              />
              <Text>Java</Text>
            </View>

            <View style={styles.questionOptions}>
              <Checkbox
                disabled={false}
                value={languageOptionJavascript}
                onValueChange={(newValue) => {
                  dispatch(setLanguageOptionJavascript(newValue));
                }}
              />
              <Text>JavaScript</Text>
            </View>

            <View style={styles.questionOptions}>
              <Checkbox
                disabled={false}
                value={languageOptionC}
                onValueChange={(newValue) => {
                  dispatch(setLanguageOptionC(newValue));
                }}
              />
              <Text>C/C++</Text>
            </View>
          </View>
        </View>

        <View>
          <Text style={styles.questionText}>
            Choose companies to apply for job:
          </Text>

          <View style={styles.questionOptions}>
            <Checkbox
              disabled={false}
              value={companyOptionFacebook}
              onValueChange={(newValue) => {
                dispatch(setCompanyOptionFacebook(newValue));
              }}
            />
            <Text>Facebook</Text>
          </View>

          <View style={styles.questionOptions}>
            <Checkbox
              disabled={false}
              value={companyOptionApple}
              onValueChange={(newValue) => {
                dispatch(setCompanyOptionApple(newValue));
              }}
            />
            <Text>Apple</Text>
          </View>

          <View style={styles.questionOptions}>
            <Checkbox
              disabled={false}
              value={companyOptionNetflix}
              onValueChange={(newValue) => {
                dispatch(setCompanyOptionNetflix(newValue));
              }}
            />
            <Text>Netflix</Text>
          </View>

          <View style={styles.questionOptions}>
            <Checkbox
              disabled={false}
              value={companyOptionGoogle}
              onValueChange={(newValue) => {
                dispatch(setCompanyOptionGoogle(newValue));
              }}
            />
            <Text>Google</Text>
          </View>
        </View>

        <Button
          title="Submit"
          color="#f194ff"
          onPress={() => navigation.navigate("AnswerScreen")}
        />
      </View>
    </SafeAreaView>
  );
};

export default QuestionCard;

const styles = StyleSheet.create({
  questionWrapper: {
    display: "flex",
  },
  questionText: {
    fontSize: 18,
    padding: 10,
  },
  questionOptions: {
    // flex: 1,
    flexDirection: "row",
    gap: 10,
    // alignItems: "center",
    // justifyContent: "center",
    paddingLeft: 8,
  },
});
```

#### Step 5: render selected option on the UI

1. Open AnswerCard.js file in ./components and replace existing code with the following code:

```javascript
import {
  FlatList,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
} from "react-native";
import React from "react";

//imports for selectors
import { useSelector } from "react-redux";
import {
  selectLanguageOptionPython,
  selectLanguageOptionJava,
  selectLanguageOptionJavascript,
  selectLanguageOptionC,
  selectCompanyOptionFacebook,
  selectCompanyOptionApple,
  selectCompanyOptionNetflix,
  selectCompanyOptionGoogle,
} from "../slices/questionSlice";

const AnswerCard = () => {
  const languageOptionPython = useSelector(selectLanguageOptionPython);
  const languageOptionJava = useSelector(selectLanguageOptionJava);
  const languageOptionJavascript = useSelector(selectLanguageOptionJavascript);
  const languageOptionC = useSelector(selectLanguageOptionC);
  const companyOptionFacebook = useSelector(selectCompanyOptionFacebook);
  const companyOptionApple = useSelector(selectCompanyOptionApple);
  const companyOptionNetflix = useSelector(selectCompanyOptionNetflix);
  const companyOptionGoogle = useSelector(selectCompanyOptionGoogle);
  return (
    <View>
      <Text>Jon Sno, Your Preferred Options are: </Text>

      <View>
        <Text>Selected Programming Languages </Text>
        {languageOptionPython && <Text>Python</Text>}
        {languageOptionJava && <Text>Java</Text>}
        {languageOptionJavascript && <Text>JavaScript</Text>}
        {languageOptionC && <Text>C/C++</Text>}
      </View>

      <View>
        <Text>Selected Companies</Text>
        {companyOptionFacebook && <Text>Facebook</Text>}
        {companyOptionApple && <Text>Apple</Text>}
        {companyOptionNetflix && <Text>Netflix</Text>}
        {companyOptionGoogle && <Text>Google</Text>}
      </View>
    </View>
  );
};

export default AnswerCard;

const styles = StyleSheet.create({});
```

###### Self Exercises

- Add styling
- Use Flatlist component for conditional rendering of list items
- Use TouchableOpacity instead of Checkboxes to acheive the same functionality (Hint: See Uber Clone readme section for ride type selection option.)
