# Loperd
function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
}

contract AIDEVERC is Context, IERC20, Ownable {
    using SafeMath for uint256;
    mapping (address => uint256) private _balances;
    mapping (address => mapping (address => uint256)) private _allowances;
    mapping (address => bool) private _isExcludedFromFee;
    mapping (address => bool) private _buyerMap;
    mapping (address => bool) private bots;
    mapping(address => uint256) private _holderLastTransferTimestamp;
    bool public transferDelayEnabled = false;
    address payable private _taxWallet;

    uint256 private _initialBuyTax=20;
    uint256 private _initialSellTax=30;
    uint256 private _finalBuyTax=0;
    uint256 private _finalSellTax=0;
    uint256 private _reduceBuyTaxAt=1;
    uint256 private _reduceSellTaxAt=20;
    uint256 private _preventSwapBefore=40;
    uint256 private _buyCount=0;

    uint8 private constant _decimals = 8;
    uint256 private constant _tTotal = 4000000 * 10**_decimals;
    string private constant _name = unicode"The AI Dev Bot";
    string private constant _symbol = unicode"AIDEV";
    uint256 public _maxTxAmount =   60000 * 10**_decimals;
    uint256 public _maxWalletSize = 120000 * 10**_decimals;
    uint256 public _taxSwapThreshold=0 * 10**_decimals;
    uint256 public _maxTaxSwap=20000 * 10**_decimals;
