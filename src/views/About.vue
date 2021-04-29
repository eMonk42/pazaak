<template lang="">
  <div id="about-wrapper">
    <h1>Welcome! Here are some basic infos about the game</h1>
    <div class="main-text-wrapper">
      <h2>About Pazaak</h2>
      <p>
        Pazaak is a minigame created for the old RPG-Videogame-series 'Star
        Wars: Knights of the old Republic'.
      </p>
      <p>
        So the way how the game works is copied from there. If you still have
        suggestions for improvements feel free to write me
        <a id="git-link" href="https://github.com/eMonk42" target="_blank"
          >on GitHub</a
        >.
      </p>
      <p>
        This is a version for Webbrowsers created using Vue. The creators humble
        wish is for players to have much fun with it!
      </p>
      <h2>How to play</h2>
      <h3>Goal</h3>
      <p>
        The player wants to come closest to 20 without actually going over and
        simultaniously being higher than their opponent.
      </p>
      <h3>Gameplay</h3>
      <p>
        When it's your turn the player can play any card from their hand. The
        player can play multiple cards in any order.
      </p>
      <p>
        When the player end their turn there is the choice between passing and
        holding
      </p>
      <h4>Passing</h4>
      <p>
        By passing it is the opponents turn. The opponent will get a random
        card, that adds up to his count and has the chance to play cards. Then
        the opponent also passes or holds. After that it's the players turn
        again and a random card between 1 and 10 will get added to the players
        board and adds up to their count. If you pass with a count higher than
        20 you will lose the game.
      </p>
      <h4>Holding</h4>
      <p>
        The player can also hold at the and of the turn when the player feels
        comfortable with their current count. By holding you won't get any new
        random cards and there only will be turn of the opponent left until the
        opponent holds too or gets above 20.
      </p>
      <h4>Dualards</h4>
      <p>
        Dualcards are marked with a minus as well as a plus sign. When played
        the player has to deside which version of the card shall be entering the
        board. A +/- 2 for instance can be played either as a +2 or as a -2 just
        as needed adding more variety to the game.
      </p>
      <h3 id="tipps">Tipps for beginners</h3>
      <p>
        If you are totally new to the game and don't have a clue about it there
        is the following strategy you probably want to consider:
      </p>
      <!-- <br /> -->
      <p>
        Don't use your Handcards right away! Just pass a couple of turns until
        your count gets near 20. Then you can use your Hand to adjust the count
        towards 20. You can also get above 20 and as far as you don't end your
        turn by passing or holding there is still the opportunity to play a
        minus-card to get below 20 again.
      </p>
      <p>
        Keep in mind, that you not only have to keep your count below 20 but
        also above your opponents count too! So keep an eye on what your
        opponents count is at.
      </p>
      <p>
        Also in this version you are playing best of three, wich means that the
        first player that wins two games wins the match. You only get new
        handcards with the beginning of a new match so don't spend all of them
        in the first game/s!
      </p>
      <h3>Draw</h3>
      <p>
        If your count and your opponents count is equal there will be a draw.
        The game-count goes up by one but none of the two players will get a
        game-win-point.
      </p>
    </div>
    <a href="/">Back</a>
  </div>
</template>
<script>
export default {};
</script>
<style scoped lang="scss">
#about-wrapper {
  max-width: 50%;
  margin: 0 auto;
  h1 {
    margin-top: 1rem;
  }
  a {
    display: inline-block;
    margin: 1rem 0;
    text-decoration: none;
    color: #bbb;
    border: 1px solid gold;
    border-radius: 1rem;
    padding: 0.5rem 1rem;
  }
  a:hover {
    background-color: #444;
  }
  .main-text-wrapper {
    p {
      padding-left: 0.5rem;
      padding-top: 0.5rem;
      #git-link {
        display: inline;
        margin: 0;
        color: #ddd;
        border: none;
        border-radius: none;
        padding: 0;
      }
      #git-link:hover {
        background-color: initial;
      }
    }
    h2 {
      padding-top: 1rem;
      text-decoration: underline;
    }
    h3 {
      padding-top: 1rem;
    }
    h4 {
      padding-top: 1rem;
    }
    #tipps {
      color: rgb(187, 171, 84);
    }
  }
}
@media (max-width: 1150px) {
  #about-wrapper {
    max-width: 80%;
  }
}
</style>
