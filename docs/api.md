# API Reference

This document outlines planned repository and service APIs.

## Repositories

- `WordRepository`
  - `getById(id)`
  - `getByIds(ids)`
  - `getByTagIds(tagIds)`
  - `save(word)`

- `GameRepository`
  - `saveState(state)`
  - `getState(id)`

## Services

- `GameLogicService`
  - `createGameState(packId)`
  - `validateSelection(...)`

- `LearningListService`
  - `addWord(wordId)`
  - `getWordsNeedingReview()`
