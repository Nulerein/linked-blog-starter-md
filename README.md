# linked-blog-starter-md
These are the markdown files for the [linked-blog-starter](https://github.com/matthewwong525/linked-blog-starter) repository

using UnityEngine;

using UnityEngine.UI;

using TMPro;

  

public class ClickerGame : MonoBehaviour

{

    public TextMeshProUGUI scoreText;

    public TextMeshProUGUI levelText;

    public TextMeshProUGUI clickUpgradeText;

    public TextMeshProUGUI autoClickUpgradeText;

    public Button clickButton;

    public Button clickUpgradeButton;

    public Button autoClickUpgradeButton;

    public Image xpBar;

  

    private int score = 0;

    private int level = 1;

    private int xp = 0;

    private int xpToNextLevel = 100;

    private int clickPower = 1;

    private int autoClickPower = 0;

    private int clickUpgradeCost = 100;

    private int autoClickUpgradeCost = 150;

  

    public Sprite[] clickButtonSprites;

  

    void Start()

    {

        LoadProgress(); // Загрузка прогресса при старте игры

        UpdateUI();

        clickButton.onClick.AddListener(OnClick);

        clickUpgradeButton.onClick.AddListener(BuyClickUpgrade);

        autoClickUpgradeButton.onClick.AddListener(BuyAutoClickUpgrade);

        InvokeRepeating("AutoClick", 1f, 1f);

    }

  

    void OnClick()

    {

        score += clickPower;

        xp += clickPower;

  

        if (xp >= xpToNextLevel)

        {

            LevelUp();

        }

  

        UpdateUI();

        SaveProgress();

    }

  

    void LevelUp()

    {

        level++;

        xp = 0;

        xpToNextLevel = Mathf.RoundToInt(xpToNextLevel * 1.5f);

  

        if (level - 1 < clickButtonSprites.Length)

        {

            clickButton.image.sprite = clickButtonSprites[level - 1];

        }

    }

  

    void BuyClickUpgrade()

    {

        if (score >= clickUpgradeCost)

        {

            score -= clickUpgradeCost;

            clickPower++;

            clickUpgradeCost *= 2;

            UpdateUI();

            SaveProgress();

        }

    }

  

    void BuyAutoClickUpgrade()

    {

        if (score >= autoClickUpgradeCost)

        {

            score -= autoClickUpgradeCost;

            autoClickPower++;

            autoClickUpgradeCost *= 2;

            UpdateUI();

            SaveProgress();

        }

    }

  

    void AutoClick()

    {

        score += autoClickPower;

        UpdateUI();

        SaveProgress();

    }

  

    void UpdateUI()

    {

        scoreText.text = "ФурриКоин: " + score;

        levelText.text = "Уровень: " + level;

        xpBar.fillAmount = (float)xp / xpToNextLevel;

        clickUpgradeText.text = $"+1 клик\n{clickUpgradeCost} фуррикоин";

        autoClickUpgradeText.text = $"+1 автоклик\n{autoClickUpgradeCost} гулей";

    }

  

    void SaveProgress()

    {

        PlayerPrefs.SetInt("Score", score);

        PlayerPrefs.SetInt("Level", level);

        PlayerPrefs.SetInt("Xp", xp);

        PlayerPrefs.SetInt("XpToNextLevel", xpToNextLevel);

        PlayerPrefs.SetInt("ClickPower", clickPower);

        PlayerPrefs.SetInt("AutoClickPower", autoClickPower);

        PlayerPrefs.SetInt("ClickUpgradeCost", clickUpgradeCost);

        PlayerPrefs.SetInt("AutoClickUpgradeCost", autoClickUpgradeCost);

        PlayerPrefs.Save();

    }

  

    void LoadProgress()

    {

        if (PlayerPrefs.HasKey("Score"))

        {

            score = PlayerPrefs.GetInt("Score");

            level = PlayerPrefs.GetInt("Level");

            xp = PlayerPrefs.GetInt("Xp");

            xpToNextLevel = PlayerPrefs.GetInt("XpToNextLevel");

            clickPower = PlayerPrefs.GetInt("ClickPower");

            autoClickPower = PlayerPrefs.GetInt("AutoClickPower");

            clickUpgradeCost = PlayerPrefs.GetInt("ClickUpgradeCost");

            autoClickUpgradeCost = PlayerPrefs.GetInt("AutoClickUpgradeCost");

  

            if (level - 1 < clickButtonSprites.Length)

            {

                clickButton.image.sprite = clickButtonSprites[level - 1];

            }

        }

    }

}