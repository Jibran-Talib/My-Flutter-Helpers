# My-Flutter-Helpers

Firebase SignUp Helpers

  try {
                    await _auth.createUserWithEmailAndPassword(
                        email: emailControler.text.toString(),
                        password: passwardControler.text.toString());

                    setState(() {
                      loading = false;
                      Utils().toastMessgae('Account Sucesfully Creadted');
                    });
                  } catch (e) {
                    setState(() {
                      loading = false;
                      Utils().toastMessgae(e.toString());
                    });
                  }

                 # For Login Use **signInWithEmailAndPassword** Line




                 


                  #Google Auth With Gmail Helpers

                    final FirebaseAuth _auth = FirebaseAuth.instance;

  final GoogleSignIn googleSignIn = GoogleSignIn();

  Future<User?> _handleSignIn() async {
    try {
      final GoogleSignInAccount? googleSignInAccount =
          await googleSignIn.signIn();
      if (googleSignInAccount != null) {
        final GoogleSignInAuthentication googleSignInAuthentication =
            await googleSignInAccount.authentication;

        final AuthCredential credential = GoogleAuthProvider.credential(
          accessToken: googleSignInAuthentication.accessToken,
          idToken: googleSignInAuthentication.idToken,
        );

        final UserCredential authResult =
            await _auth.signInWithCredential(credential);
        final User? user = authResult.user;
        userUid = authResult.user!.uid;
        String? userName = authResult.user!.displayName;

        SharedPreferences sp = await SharedPreferences.getInstance();
        sp.setString('uid', userUid);
        sp.setBool('checkuser', true);
        sp.setString('username', userName.toString());

        setState(() {});

        return user;
      }
    } catch (error) {
      print(error);
    }
    return null;
  }





#StreamBuilder Process :

Expanded(
          child: StreamBuilder(
            stream:
                FirebaseFirestore.instance.collection("posts").snapshots(),
            builder: (context,
                AsyncSnapshot<QuerySnapshot<Map<String, dynamic>>>
                    snapshot) {
              if (snapshot.connectionState == ConnectionState.waiting) {
                return const Center(
                  child: CircularProgressIndicator(),
                );
              }

              if (snapshot.hasData) {
                return ListView.builder(
                  itemCount: snapshot.data!.docs.length,
                  itemBuilder: (context, index) {
                    log(snapshot.data!.docs[index].data().toString());

                    return UserVideos(
                      snap: snapshot.data!.docs[index].data(),
                    );
                  },
                );
              }
              log("${snapshot.stackTrace}stack trace");
              log("${snapshot.error}error");
              return const Text("Something went wrong");
            },
          ),
        ),





#Provider Helpers

import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (context) => Counter(), // Provide the Counter object
      child: MaterialApp(
        title: 'Flutter Provider Example',
        home: CounterApp(),
      ),
    );
  }
}

// Our Counter class which extends ChangeNotifier
class Counter extends ChangeNotifier {
  int _count = 0;

  int get count => _count;

  void increment() {
    _count++;
    notifyListeners(); // Notify listeners about the change
  }

  void decrement() {
    _count--;
    notifyListeners(); // Notify listeners about the change
  }
}

class CounterApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Counter App'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'Counter Value:',
            ),
            Consumer<Counter>(
              builder: (context, counter, child) => Text(
                '${counter.count}',
                style: TextStyle(fontSize: 24),
              ),
            ),
          ],
        ),
      ),
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
            onPressed: () {
              Provider.of<Counter>(context, listen: false).increment();
            },
            tooltip: 'Increment',
            child: Icon(Icons.add),
          ),
          SizedBox(height: 10),
          FloatingActionButton(
            onPressed: () {
              Provider.of<Counter>(context, listen: false).decrement();
            },
            tooltip: 'Decrement',
            child: Icon(Icons.remove),
          ),
        ],
      ),
    );
  }
}


#Api Helper With Array Name

  Future<List<ApiDataModle>> getApiData() async {
    final response =
        await http.get(Uri.parse("https://jsonplaceholder.typicode.com/users"));
    var data = jsonDecode(response.body.toString());

    if (response.statusCode == 200) {
      for (Map<String, dynamic> i in data) {
        apiDataList.add(ApiDataModle.fromJson(i));
      }

      return apiDataList;
    } else {
      return apiDataList;
    }
  }


  body: Column(
          children: [
            Expanded(
              child: FutureBuilder(
                future: getApiData(),
                builder: (context, AsyncSnapshot<List<ApiDataModle>> snapshot) {
                  if (!snapshot.hasData) {
                    return Center(child: CircularProgressIndicator());
                  } else {
                    return ListView.builder(
                      itemCount: apiDataList.length,
                      itemBuilder: (context, index) {
                        return Padding(
                          padding: const EdgeInsets.all(8.0),
                          child: Card(
                            child: Padding(
                              padding: const EdgeInsets.all(8.0),
                              child: Column(
                                children: [
                                  Row(
                                    mainAxisAlignment:
                                        MainAxisAlignment.spaceBetween,
                                    children: [
                                      Text("Name"),
                                      Text(snapshot.data![index].name
                                          .toString()),
