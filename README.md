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

                  For Login Use **signInWithEmailAndPassword** Line
