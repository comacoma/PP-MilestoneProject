# Practical Python Milestone Project
<details><summary>Details</summary>

- The aim of this project is to build a web application game that asks players to guess the answer to a pictorial or text-based riddle.
- The player is presented with an image or text that contains the riddle.
- Players enter their answer into a textarea and submit their answer using a form.
- If a player guesses correctly, they are redirected to the next riddle. If a player guesses incorrectly, their incorrect guess is stored and printed below the riddle. The textarea is cleared so they can guess again.
- Multiple players can play an instance of the game at the same time. Users are identified by a unique username.
- A leaderboard is also available for users to check their top scores and how they fare against other players.

</details>

*****

## Technical details

<details><summary>Details</summary>
- The Application will be styled with Bootstrap based theme and aiming for a responsive design.
- Riddles will be stored as JSON file. Each "riddle" will consists of the riddle - either as a string if it is text-based, or a URL to an image of it is pictorial. Ability to add new riddles from client side is not part of the requirement so should the riddle base requires update, they will be added to the JSON file directly.
- Due to requirement of displaying users scores, usernames and their scores will have to be stored in a file (likely on JSON format) on the server. Although this requirement can be achieved without having to store the data in a file (i.e. store data in a dictionary at runtime), such data will be lost if the application crashes. It is therefore more preferable to store these data in a file.
- For project purpose, the riddle base will be relatively small for example, 10 questions in total.
- However, as for incorrect answers to a riddle; it would be sufficient to store them only at runtime.
- As part of the requirement, the application allows multiple users to play the game at the same time whilst they are identified by a unique username. Because of this, the application will need to be able to identify when a user is trying to start the game with a username that is currently active and provide them with useful warning and guidance.
- Here is a rough representation of the flow:
  1. Users are prompted to enter their username. If the username entered is currently active a warning message will appear and promotes them to enter a new username.
  2. After logging in successfully, users will be provide a choice start the game right away or take a look at the leaderboard. At this point, it has been decided to have the actual game and leaderboard displayed separately, hence the need to let users choose what to do. This might change later on (i.e. have the game and leader board displayed at the same time so there will be no need to choose).
  3. As in requirement, users proceed to answer the riddles (retrieved from JSON file) with textarea provided.
    - If users answer correctly, they will be directed to the next riddle. For a added challenge to the users, the next riddle should be retrieved at random instead of the having the same order of riddles every time.
    - If users answer incorrectly, the answer will be stored in a list (only available at runtime) and displayed underneath the riddle.
    - As a extra option not specified in requirement, if users struggle with a riddle but would like to continue with the game, they are given the choice to pass. This can be implemented in forms of a button click or have the user send an answer consists of "nothing" (i.e. empty string).
  4. Users can quit the game at any time at which point the flow will start again from step 1.
- The application will follow a test-driven development strategy. Any details regarding testing will be provided [HERE](#Testing)
- Flask framework will be used and the application will be deployed using Heroku.

</details>

*****

## Change Log

### 14/07/2018
- New player login implemented.
    - Player login will now require password on top of player name.
    - Basic form validation are handled in frontend.
    - Flask session has been used for handling players login.
- New game logic implemented.
    - Each game will now be finite instead of running on forever until players decide to quit. A single game will go through all questions in the riddle base and no question will be repeated within the same game.
    - Players can attempt to go through the whole game or drop out at anytime at which, scores will be calculated.
    - Current scores is only relevant when player is playing, hence it will not be stored in data file. Top score otherwise, will update when players make a new record for themselves.
- Added 'rank' column to leaderboard for better readability.

#### 23:31
- Hint feature implemented.

### 11/07/2018
After review the application will undergo some hugh changes, they are as follow:
- Player login will require password.
- Form validation such as field length limitation for better user friendliness.
- The application will loop through the whole riddle base (in random order) instead of picking one at random. This will prevent questions from repeating too often.
- Along with the change above, instead of a accumulative score, there will be a top score and a current score for each player. That means each session will be treated individually.
- Implementation of hints to help players who are struggling.
- Further styling on pictorial riddles so they are easier to look at on larger screens.
- Change layout of riddle page.

Anything not mentioned in this list will remain mostly the same.

### 04/07/2018
- Deployed on Heroku.
- Added requirements.txt and Procfile needed for deployment on Heroku.
- Added .gitignore file.
- Added fallback solution when application is used for the very first time where players.json and active_players.json do not exist yet.

### 01/07/2018
- Follow up to [THIS](#30062018), an intermediate route was added to process player login and as a result player can no longer directly go to player page by entering the URL (/player/<player_name>). With this change, a new stray.html template will be used in case player attempts to access the application directly with URL.

### <a name="30062018"></a>30/06/2018
- Further improve routing so that player can log in directly by entering the correct URL.
- Added warning message when player enter a name that is currently being used when attempting to log in.
- Added button to go back to player page when leaderboard is accessed from there.

### 27/06/2018
- leaderboard.html implemented.
- Further improve routing to accommodate the addition of leaderboard.
- Added test class related to leaderboard.
- Updated testRoute test class to reflect new routing.

### 25/06/2018
- Functionality related to riddles processing has been implemented in full (checks for correct answer and update score), except for simultaneous use case scenario.

### 24/06/2018
- Added player options to player.html in form of buttons.
- Player login now also does uniquesness check so that no duplicates of player name are allowed in player.json
- riddles.html template implemented.

#### 21:04
- riddles.html will now display riddle with answer text space below. A new riddle will be chosen randomly on each load.
- Improved routing on riddles

### 23/06/2018
- Partial implementation of player.html
- Player login function implemented (without uniqueness check)

#### 14:26
- Template output for player.html implemented. This template will show when user attemp to change route/url directly instead of using form to login.

### 20/06/2018
- Riddles base (as json file) has been added. A new attribute "type" is added to each riddle. This is due to the fact that the application needs to identify what type of riddle has been chosen and use appropriate HTML to display the riddle correctly.

#### 15:21
- Added HTML templates.
- index.html implemented (UI only). 
- Added run.py (init script for the application) and its test script test_run.py.


*****

## <a name="Testing"></a>Testing

### Data manuipluation with player.json
For a starter the following pseudo code is used to test if data written to player.json as expected.
```python
def write_to_player_json():
    player = {"player_name": "dummy_player", "score": 0}
    with open('data/player.json') as f:
        data = json.load(f)
    
    data.append(player)
    
    with open('data/player.json', 'w') as f:
        json.dump(data, f)
```
And to test it:
```python
def test_write_to_player_json(self):
    with open('data/player.json', 'w') as f:
        json.dump([], f)
    for x in range(5):
        write_to_player_json()
    with open('data/player.json', 'r') as f:
        data = json.load(f)
        self.assertEqual(len(data), 5)
```
The reason why the function was called multiple times is to make sure new data is appended to player.json instead of overwritting the whole file. The function will be updated to take two parameters of player_name and file_name instead of hard-coded data.

As the function to identify whether or not a player name entered exists in player.json has been implemented, further tests has been put in place to ensure that the application will not create duplicates of player record in player.json. This serves as the first step of implementing the requirement "Multiple players can play an instance of the game at the same time" where players are identified by a unique player name.

### Logic of processing riddles
Players are instructed that if they want to pass a riddle, they can do so by submitting a blank answer. This will also in turn saves the effort of implementing code to check "whether a blank answer is the correct answer" or possible bug that may arise by submitting a blank answer. What the application does actually is that when player submits a blank answer, the page reloads and display a new riddle, nothing else. 

As for handling POST request, the code will look something like this:
```python
if request.method == "POST":
        if pass_riddle(request.form["answer"]):
            return render_template("riddles.html", player=player_detail, riddle=riddle)
        else:
            print("checking answer")
```
At this stage it might be better to test the result manually rather writting tests since all that is needed it to check if the string "checking answer" printed onto the console when something is entered into the text space.

That said, it would be reasonable to write tests to test the behaviour of the overall logic of processing riddles once the code is in a more complete state. The elementes that need to be tested are:
- Identifying whether player has submitted a correct answer or not.
- Updating scores.
- Displaying wrong answers as per requirements.

### Sorting out player scores and displaying on leaderboard.html
First of all, leaderboard.html can be reached via 2 different routes:
- /leaderboard
- /player/<player_name>/leaderboard

As far as routing test is concerned, both route needs to be tested independently.

On the other hand, in order to simplify testing for sorting scores and displaying them on leaderboard.html, a preset of data is used as follows:
```python
test_data = [
            {"score": 0, "player_name": "a"}, 
            {"score": 5, "player_name": "b"}, 
            {"score": 3, "player_name": "c"}, 
            {"score": 2, "player_name": "d"}]
write_json('data/player.json', test_data)
```
where write_json is a function from run.py for writing data directly into player.json
```python
def write_json(file_name, data):
    with open(file_name, 'w') as f:
        json.dump(data, f)
```
As for actually testing whether scores are sorted and displayed correctly, we can check where does certain data exist in the html document and see if certain elements exists before some other. This can be achieved by first converting html document into string and use .index() to find out location of certain elements, which will then be used for comparison to check if order is correct.

### Multiple users use case scenario
Since it was not mentioned in the requirement that user authentication is required, as long as all current players within the instance are unique to each other (identified by their player name) it would suffice. This can be tested by submitting multiple log in request with the same player name. Once the first request has been processed the application should not allow further log in attempts with the same name until the said name is no longer active.

### Finalizing
With the implementation of /stray route to handle direct access with URL, some routing tests have to be modified in order to better illustrate how the application is expected to work. For example:
```python
def test_player_loads(self):
    tester = app.test_client(self)
    response = tester.get('player/dummy_player')
    self.assertEqual(response.status_code, 200)
    self.assertTrue(b'Welcome, dummy_player' in response.data)
```
This code was used to test if player.html loads however, with the implementation of /stray route this test will return an assertion error was the status code returned is 302 (redirected) rather than 200. The reason for this is because if a user enters /player/player_name without using the form in index.html then the application will redirects to the stray.html, hence the status code 302.

As such, to better illustrate how the application is expected to work, one more line of code is needed in this test:
```python
tester.post('/', data=dict(player_name = 'dummy_player'), follow_redirects = True)
```
This concept is applicable in several tests as well; so instead of injecting test data directly into data file, they are posted through a test client as how a normal user would do.

### Changes to tests along with rework
With the rework mentioned in change log, there is also a need to adjust tests so that they are still relevant. Here is a list of the tests changes:

#### Player login
Since it also require password on top of player name to login, test data needs to change from {player_name: 'dummy_player'} to {player_name: 'dummy_player', password: 'dummy_player'} to reflect that.

#### Test with Flask session in mind
With the implementation of Flask session, it is necessary to keep in mind that tests should be carried out with a session available. Because of that, there is a need to set a secret key before each test to make sure a session is created properly.

One such example is test_riddles_loads, session variables needs to be set up manually so a simple get request without setup in the test will return the stray.html page. This is due to the fact that correct page will only load if a session exists. To manually set session variables, the following code is used:
```python
with self.app.session_transaction() as sess:
    sess['player'] = 'dummy_player'
    sess['qs'] = [str(x) for x in range(len(read_json('data/riddles.json')))]
    sess['wrong_answers'] = []
    sess['current_score'] = 0
```

#### New tests for game logic
As the game logic has changed vastly, there is a need to add new tests to make sure the new game logic works for example, a test is needed to check if the game actually loops through all the question without repeating and ends the game when it has gone through all questions.

The test for game logic does not actually goes through all question as if the application is actually running but rather shows the logic behind the game loop and test is end result (redirected back to player page) is as expected.

To start off, before player starts a game, a list is stored into session. This list is to keep track of how many questions left. Whenever player proceeds (either by passing or by submitting the correct answer), the key representing that question is removed from the list before another key is selected ay random. This allows players to go through all questions in the riddle base without having to repeat any question. Here is a screenshot below to explain the logic visually.

![Visual representation of the logic behind game loop](static/image/test-img-1.jpg)

