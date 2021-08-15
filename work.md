## Featured Project

One project of mine that I enjoyed working on was the Apple Pie project. This project was a guided project from Apple's "Develop in Swift Fundamentals." The objecive of the lab was to create an iPad application where the user guessed a word. If the user chose a letter that was a part of the word, the blanks would update with the letter in the letter's correct position or positions, and if the user guessed the letter incorrectly, an apple would fall from the apple tree, indicating that an attempt has been lost. 

The first image below shows the start scene after the first round has started. Displayed below is the number of wins and losses, the word's text blanks, the letters to choose from, and the tree which indicates how many lives the user has left.

<img src="https://user-images.githubusercontent.com/88736917/129485267-4ed38bf1-7acf-45c4-a4f2-9d78bcdfb020.jpg" width="500">

After entering a few letters, we can see how the interface updates the letters chosen and darkens the letter button so that the user cannot select it again.

<img src="https://user-images.githubusercontent.com/88736917/129485462-6bd21783-a41b-4370-95c3-655caddf948b.jpg" width="500">

After a round is won, the wins counter at the bottom updates by 1. If a round is lost, then the loss counter would update by 1.

<img src="https://user-images.githubusercontent.com/88736917/129485498-92d13468-a301-4de6-adf8-beffa7eb5b0d.jpg" width="500">

The final image below shows some letters selected correctly, but also shows a couple of apples missing from the tree, indicating that the user has selected several incorrect answers.

<img src="https://user-images.githubusercontent.com/88736917/129485519-d9569d9b-19a6-4d3c-af78-322f0f2847f0.jpg" width="500">

Here is the code that was used for this project:

```markdown

//
//  ViewController.swift
//  Apple Pie
//
//  Created by Ian Harris on 8/9/21.
//

import UIKit

class ViewController: UIViewController {

    var listOfWords = ["buccaneer", "swift", "glorious", "incandescent", "bug", "program"]
    let incorrectMovesAllowed = 7
    var totalWins = 0 {
        didSet {
            newRound()
        }
    }
    var totalLosses = 0 {
        didSet {
            newRound()
        }
    }
    
    @IBOutlet var treeImageView: UIImageView!
    @IBOutlet var correctWordLabel: UILabel!
    @IBOutlet var scoreLabel: UILabel!
    @IBOutlet var letterButtons: [UIButton]!
    
    
    override func viewDidLoad() {
        super.viewDidLoad()
        newRound()
        // Do any additional setup after loading the view.
    }
    
    var currentGame: Game!
    
    func newRound() {
      if !listOfWords.isEmpty {
        let newWord = listOfWords.removeFirst()
        currentGame = Game(word: newWord, incorrectMovesRemaining: incorrectMovesAllowed, guessedLetters: [])
            enableLetterButtons(true)
        updateUI()
        } else {
            enableLetterButtons(false)
        }
    }

    func enableLetterButtons(_ enable: Bool) {
        for button in letterButtons {
            button.isEnabled = enable
        }
    }
    
    func updateUI() {
        var letters = [String]()
        for letter in currentGame.formattedWord {
            letters.append(String(letter))
        }
        let wordWithSpacing = letters.joined(separator: " ")
        correctWordLabel.text = wordWithSpacing
        scoreLabel.text = "Wins: \(totalWins), Losses: \(totalLosses)"
        treeImageView.image = UIImage(named: "Tree \(currentGame.incorrectMovesRemaining)")
    }
    
    @IBAction func letterButtonPressed(_ sender: UIButton) {
        sender.isEnabled = false
        let letterString = sender.title(for: .normal)!
        let letter = Character(letterString.lowercased())
        currentGame.playerGuessed(letter: letter)
        updateGameState()
    }
    
    func updateGameState() {
        if currentGame.incorrectMovesRemaining == 0 {
            totalLosses += 1
        } else if currentGame.word == currentGame.formattedWord {
            totalWins += 1
        } else {
            updateUI()
        }
    }
}

//Ian Harris - 08/09/2021

```
