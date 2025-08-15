## Why VSChart

  Recently I've been working on testing a couple of ideas on stock price analysis using Python.

  I need to visually verify the candlestick charts to see whether my program is working as expected.

  I tried a few plotting packages in Python, but none of them works as gracefully as TradingView.

  AND I do all the coding, tweaking, and debugging in Trae/VS Code.

  ------------------

  I decided to code this VS Code extension using the open source version "lightweight-charts" from TradingView, so that I can look at my codes and the chart at the same time in the same IDE window.

  The lightweight-charts provides a very nice and interactive chart with necessary basic functions, as well as an interface for user - customizable plugins.

  With that, I integrated a few useful plugins from lightweight-charts plugin examples that are available in its github repo.

  I also added a few new plugins that I find useful.

  Now, with this, I can happily work on my thing... Hope this helps you too.

## VS Code installation and usage
  Install the .vsix to your VS Code or Trae or Cursor or any other VS Code - based IDE.

  On Mac, press Command + Shift + p and then select "Open VSChart".

  For Windows, use Ctrl + Shift + p instead.

## Install the python package

```shell
pip install vschart - python
#get tests/* from github and run to see how it works
cd tests
python test_all.py
```
