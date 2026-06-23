1. Tầm quan trọng của thứ tự ưu tiên tính toán trong nghiệp vụ kế toán/tài chínhTrong các hệ thống E-commerce và FinTech, việc thiết lập sai thứ tự ưu tiên (Sequence of Operations) của các quy tắc tính toán là một trong những lỗi nghiêm trọng nhất, dẫn đến hai hậu quả lớn:Rủi ro pháp lý và Thuế (Tax Compliance): Thuế VAT phải được tính trên giá trị phân phối cuối cùng mà người tiêu dùng thực tế phải trả cho hàng hóa/dịch vụ (sau khi đã trừ tất cả các khoản chiết khấu thương mại, giảm giá, coupon). Nếu tính thuế trước khi giảm giá, doanh nghiệp sẽ khai khống doanh thu tính thuế, nộp thừa thuế và gây thiệt hại cho khách hàng. Ngược lại, nếu tính sai có thể dẫn đến trốn thuế vô ý.Thất thoát tài chính (Margin Erosion): Áp dụng giảm giá dựa trên % (như mã giảm giá 10% hay hạng thành viên 5%) trên các cơ sở giá trị khác nhau sẽ cho ra kết quả hoàn toàn khác nhau. Quy trình chuẩn là: Giảm giá trực tiếp trên sản phẩm $\rightarrow$ Giảm giá trên tổng đơn (Coupon) $\rightarrow$ Giảm giá theo đối tượng (Loyalty). Nếu đảo ngược (ví dụ: tính Loyalty trước Coupon), số tiền giảm giá tổng thể có thể bị phóng đại, trực tiếp bào mòn biên lợi nhuận của doanh nghiệp.2. Thiết kế Prompt Chain-of-Thought (CoT)Dưới đây là cấu trúc prompt được tối ưu hóa để ép AI phải suy luận từng bước một cách chặt chẽ trước khi đưa ra code Java.MarkdownBạn là một Chuyên gia Kỹ thuật Phần mềm cấp cao (Senior Backend Engineer) kiêm Chuyên gia Phân tích Nghiệp vụ Tài chính (BA FinTech).
   Nhiệm vụ của bạn là phân tích nghiệp vụ tính toán đơn hàng cho hệ thống SpeedyCart và triển khai bằng mã nguồn Java.

Hãy thực hiện nghiêm ngặt theo tư duy chuỗi suy luận (Chain-of-thought) qua các bước sau:

### BƯỚC 1: PHÂN TÍCH LOGIC VÀ THIẾT LẬP THỨ TỰ TÍNH TOÁN
Hãy phân tích thứ tự ưu tiên của các quy tắc: Tổng tiền gốc, Product Discount, Coupon Code, Loyalty Discount, VAT, và Shipping Fee.
Giải thích rõ tại sao thứ tự đó lại đúng về mặt nghiệp vụ kế toán và luật thuế.

### BƯỚC 2: XÂY DỰNG CÔNG THỨC CHI TIẾT
Thiết lập công thức toán học chính xác cho từng bước (từ Bước 1 đến bước cuối cùng). Xác định rõ điều kiện biên (ví dụ: mức trần giảm giá của coupon, điều kiện miễn phí vận chuyển).

### BƯỚC 3: CHẠY THỬ BẰNG TAY (DRY-RUN) VỚI DATA TEST
Thực hiện tính toán từng bước bằng số liệu cụ thể sau (Ghi rõ kết quả trung gian của từng bước):
- Sản phẩm A: Số lượng 2, Giá gốc 400,000 VND/sp, Giảm trực tiếp 10%.
- Sản phẩm B: Số lượng 1, Giá gốc 300,000 VND/sp, Không giảm giá.
- Mã Coupon: Giảm 10% (Tối đa 100,000 VND), áp dụng cho đơn sau giảm giá sản phẩm đạt tối thiểu 500,000 VND.
- Hạng thành viên: Gold (Giảm 5% trên tổng tiền sau coupon).
- Khoảng cách giao hàng: 12km.
  (Quy tắc ship: <10km là 30,000 VND, mỗi km tiếp theo +5,000 VND. Miễn phí ship nếu tổng tiền sau chiết khấu và trước thuế > 1,000,000 VND).

### BƯỚC 4: TRIỂN KHAI SOURCE CODE JAVA
Dựa trên các bước logic trên, hãy sinh class `OrderCalculator` với phương thức `calculateTotal(Order order)`.
Yêu cầu kỹ thuật:
1. Sử dụng KIỂU DỮ LIỆU `BigDecimal` cho tất cả các phép tính liên quan đến tiền tệ.
2. Áp dụng `RoundingMode.HALF_UP` (làm tròn thương mại) khi chia hoặc tính phần trăm.
3. Code sạch, có comment giải thích tương ứng với các bước tư duy ở trên. Hãy giả định hoặc tự định nghĩa cấu trúc cấu thành của các class phụ trợ như `Order`, `OrderItem`, `LoyaltyRank` nếu cần.
3. Minh chứng chạy thực tế (AI Response Log)Dưới đây là toàn bộ phản hồi logic, kết quả dry-run và mã nguồn Java được tạo ra từ Prompt CoT trên.BƯỚC 1: PHÂN TÍCH LOGIC VÀ THIẾT LẬP THỨ TỰ TÍNH TOÁNĐể đảm bảo tính hợp pháp về thuế và tính chính xác về mặt tài chính, thứ tự thực hiện bắt buộc phải tuân theo quy trình tuyến tính giảm dần của dòng tiền như sau:Tính tổng tiền gốc (Gross Amount): Là tổng giá trị chưa qua xử lý của toàn bộ mặt hàng.Áp dụng Giảm giá sản phẩm (Product Discount): Đây là giảm giá trực tiếp trên giá bán lẻ của sản phẩm (giá khuyến mãi tại quầy). Kết quả bước này cho ra Tổng tiền sau giảm giá sản phẩm (Cơ sở để xét điều kiện áp dụng Coupon).Áp dụng Mã giảm giá (Coupon Code): Áp dụng trên tổng giá trị hàng hóa thực tế sau khi đã trừ giảm giá sản phẩm. Không áp dụng trên giá gốc ban đầu để tránh "giảm giá chồng giảm giá" quá mức.Áp dụng Chiết khấu thành viên (Loyalty Discount): Tính trên số tiền cuối cùng khách hàng phải trả cho phần hàng hóa (sau Coupon). Đây là ưu đãi cá nhân hóa, áp dụng ở bước cuối cùng trước thuế.Xác định Phí vận chuyển (Shipping Fee): Phí ship là dịch vụ ngoài gia tăng. Cần tính toán dựa trên khoảng cách và kiểm tra điều kiện miễn phí ship dựa trên giá trị đơn hàng thu được ở Bước 4 (Sau chiết khấu, trước thuế).Tính Thuế VAT (10%): Thuế giá trị gia tăng được tính trên tổng giá trị ròng của hàng hóa mà người tiêu dùng thụ hưởng. Do đó, VAT được tính bằng: $\text{VAT} = (\text{Tổng tiền sau Loyalty}) \times 10\%$.Lưu ý nghiệp vụ: Theo luật thuế thông thường, phí vận chuyển có thể có mức thuế suất khác hoặc không chịu thuế chung với hàng hóa tùy quốc gia. Ở đây, theo bối cảnh đề bài: "Thuế VAT 10% tính trên tổng tiền sau khi đã trừ tất cả các chiết khấu trên", nghĩa là VAT chỉ đánh trên tiền hàng sau giảm giá, phí ship cộng vào sau cùng và không chịu thuế chồng thuế.Tính Tổng số tiền cuối cùng (Final Total): Tổng tiền sau Loyalty + Thuế VAT + Phí vận chuyển.BƯỚC 2: XÂY DỰNG CÔNG THỨC CHI TIẾTBước 1: Tổng tiền gốc ($T_{\text{gốc}}$)$$T_{\text{gốc}} = \sum (\text{Giá gốc}_i \times \text{Số lượng}_i)$$Bước 2: Tổng tiền sau giảm giá sản phẩm ($T_{\text{sp}}$)$$T_{\text{sp}} = \sum \left[ \text{Số lượng}_i \times (\text{Giá gốc}_i \times (1 - \% \text{Giảm sản phẩm}_i)) \right]$$Bước 3: Tổng tiền sau Coupon ($T_{\text{coupon}}$)Điều kiện: Nếu $T_{\text{sp}} \ge 500,000$, tiến hành tính $V_{\text{coupon}} = T_{\text{sp}} \times 10\%$.Giới hạn trần: Nếu $V_{\text{coupon}} > 100,000 \rightarrow V_{\text{coupon}} = 100,000$.$$T_{\text{coupon}} = T_{\text{sp}} - V_{\text{coupon}}$$(Nếu không đủ điều kiện, $T_{\text{coupon}} = T_{\text{sp}}$).Bước 4: Tổng tiền sau chiết khấu thành viên ($T_{\text{loyalty}}$)Hạng Gold: $T_{\text{loyalty}} = T_{\text{coupon}} \times (1 - 0.05)$Hạng Platinum: $T_{\text{loyalty}} = T_{\text{coupon}} \times (1 - 0.10)$Bước 5: Phí vận chuyển ($P_{\text{ship}}$)Nếu $T_{\text{loyalty}} > 1,000,000 \rightarrow P_{\text{ship}} = 0$.Ngược lại:Nếu $\text{Khoảng cách} \le 10\text{km} \rightarrow P_{\text{ship}} = 30,000$.Nếu $\text{Khoảng cách} > 10\text{km} \rightarrow P_{\text{ship}} = 30,000 + (\text{Khoảng cách} - 10) \times 5,000$.Bước 6: Thuế VAT$$\text{VAT} = T_{\text{loyalty}} \times 10\%$$Bước 7: Tổng thanh toán cuối cùng ($\text{Total}$)$$\text{Total} = T_{\text{loyalty}} + \text{VAT} + P_{\text{ship}}$$BƯỚC 3: CHẠY THỬ BẰNG TAY (DRY-RUN)Dữ liệu đầu vào:Sản phẩm A: Số lượng = 2; Giá gốc = 400,000 VND; Giảm trực tiếp = 10%Sản phẩm B: Số lượng = 1; Giá gốc = 300,000 VND; Giảm trực tiếp = 0%Mã coupon: Giảm 10%, tối đa 100,000 VND cho đơn đạt tối thiểu 500,000 VNDHạng: Gold (Giảm 5%)Khoảng cách: 12kmTiến trình tính toán chi tiết:Tính tổng tiền sau giảm giá sản phẩm ($T_{\text{sp}}$):Sản phẩm A: $2 \times (400,000 \times (1 - 0.10)) = 2 \times 360,000 = 720,000\text{ VND}$.Sản phẩm B: $1 \times 300,000 = 300,000\text{ VND}$.$\rightarrow T_{\text{sp}} = 720,000 + 300,000 = 1,020,000\text{ VND}$.Áp dụng Coupon ($T_{\text{coupon}}$):Kiểm tra điều kiện đơn hàng tối thiểu: $T_{\text{sp}} = 1,020,000 \ge 500,000$ $\rightarrow$ Thỏa mãn.Số tiền giảm theo lý thuyết: $1,020,000 \times 10\% = 102,000\text{ VND}$.Kiểm tra mức giảm tối đa: $102,000 > 100,000 \rightarrow$ Số tiền giảm thực tế cố định ở mức trần là $100,000\text{ VND}$.$\rightarrow T_{\text{coupon}} = 1,020,000 - 100,000 = 920,000\text{ VND}$.Áp dụng Chiết khấu thành viên Gold ($T_{\text{loyalty}}$):Số tiền giảm hạng Gold: $920,000 \times 5\% = 46,000\text{ VND}$.$\rightarrow T_{\text{loyalty}} = 920,000 - 46,000 = 874,000\text{ VND}$.Tính Phí vận chuyển ($P_{\text{ship}}$):Kiểm tra điều kiện miễn phí ship: $T_{\text{loyalty}} = 874,000 \le 1,000,000 \rightarrow$ Không được miễn phí ship.Khoảng cách là 12km (> 10km).$P_{\text{ship}} = 30,000 + (12 - 10) \times 5,000 = 30,000 + 10,000 = 40,000\text{ VND}$.Tính Thuế VAT:$\text{VAT} = 874,000 \times 10\% = 87,400\text{ VND}$.Tính Tổng số tiền cuối cùng ($\text{Total}$):$\text{Total} = T_{\text{loyalty}} + \text{VAT} + P_{\text{ship}} = 874,000 + 87,400 + 40,000 = 1,001,400\text{ VND}$.Kết quả Dry-run: 1,001,400 VND.BƯỚC 4: TRIỂN KHAI SOURCE CODE JAVADưới đây là cấu trúc các Class và mã nguồn tính toán được tối ưu hóa bằng BigDecimal.Javapackage com.speedycart.promotion;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.List;

// --- ĐỊNH NGHĨA CÁC ĐỐI TƯỢNG DATA CƠ BẢN ---

enum LoyaltyRank {
NONE, SILVER, GOLD, PLATINUM
}

class OrderItem {
private String productName;
private BigDecimal basePrice;
private int quantity;
private double productDiscountPercent; // Ví dụ: 10% truyền vào là 10.0 hoặc 0.10. Ở đây tính theo % (0 đến 100)

    public OrderItem(String productName, BigDecimal basePrice, int quantity, double productDiscountPercent) {
        this.productName = productName;
        this.basePrice = basePrice;
        this.quantity = quantity;
        this.productDiscountPercent = productDiscountPercent;
    }

    public BigDecimal getBasePrice() { return basePrice; }
    public int getQuantity() { return quantity; }
    public double getProductDiscountPercent() { return productDiscountPercent; }
}

class Order {
private List<OrderItem> items;
private boolean hasCoupon;
private LoyaltyRank loyaltyRank;
private double shippingDistanceKm;

    public Order(List<OrderItem> items, boolean hasCoupon, LoyaltyRank loyaltyRank, double shippingDistanceKm) {
        this.items = items;
        this.hasCoupon = hasCoupon;
        this.loyaltyRank = loyaltyRank;
        this.shippingDistanceKm = shippingDistanceKm;
    }

    public List<OrderItem> getItems() { return items; }
    public boolean isHasCoupon() { return hasCoupon; }
    public LoyaltyRank getLoyaltyRank() { return loyaltyRank; }
    public double getShippingDistanceKm() { return shippingDistanceKm; }
}

// --- CLASS XỬ LÝ TÍNH TOÁN CHÍNH ---

public class OrderCalculator {

    private static final BigDecimal HUNDRED = new BigDecimal("100");
    private static final BigDecimal VAT_RATE = new BigDecimal("0.10"); // 10%
    
    // Ngưỡng cấu hình Coupon
    private static final BigDecimal COUPON_MIN_ORDER_VAL = new BigDecimal("500000");
    private static final BigDecimal COUPON_MAX_DISCOUNT = new BigDecimal("100000");
    private static final BigDecimal COUPON_DISCOUNT_PERCENT = new BigDecimal("0.10");

    // Ngưỡng cấu hình Shipping
    private static final BigDecimal FREE_SHIP_THRESHOLD = new BigDecimal("1000000");
    private static final BigDecimal BASE_SHIP_FEE = new BigDecimal("30000");
    private static final BigDecimal EXTRA_SHIP_FEE_PER_KM = new BigDecimal("5000");
    private static final double BASE_SHIP_DISTANCE_LIMIT = 10.0;

    public BigDecimal calculateTotal(Order order) {
        // LƯU Ý: Khởi tạo tất cả biến tính toán bằng BigDecimal
        
        // Bước 1 & 2: Tính tổng tiền sau khi áp dụng giảm giá trực tiếp trên từng sản phẩm
        BigDecimal totalAfterProductDiscount = BigDecimal.ZERO;

        for (OrderItem item : order.getItems()) {
            BigDecimal itemBasePrice = item.getBasePrice();
            BigDecimal quantity = new BigDecimal(item.getQuantity());
            
            // Tính số tiền giảm giá của 1 sản phẩm
            BigDecimal discountPercent = BigDecimal.valueOf(item.getProductDiscountPercent());
            BigDecimal discountMultiplier = discountPercent.divide(HUNDRED, 4, RoundingMode.HALF_UP);
            BigDecimal itemDiscountAmount = itemBasePrice.multiply(discountMultiplier);
            
            // Giá thực tế của 1 sản phẩm sau giảm giá
            BigDecimal actualItemPrice = itemBasePrice.subtract(itemDiscountAmount);
            
            // Cộng dồn vào tổng tiền hàng
            BigDecimal totalItemAmount = actualItemPrice.multiply(quantity);
            totalAfterProductDiscount = totalAfterProductDiscount.add(totalItemAmount);
        }

        // Bước 3: Áp dụng Mã giảm giá (Coupon Code)
        BigDecimal totalAfterCoupon = totalAfterProductDiscount;
        if (order.isHasCoupon()) {
            // Kiểm tra điều kiện đạt giá trị tối thiểu 500,000 VND
            if (totalAfterProductDiscount.compareTo(COUPON_MIN_ORDER_VAL) >= 0) {
                BigDecimal couponDiscount = totalAfterProductDiscount.multiply(COUPON_DISCOUNT_PERCENT);
                // Áp dụng cơ chế cắt trần giảm giá tối đa 100,000 VND
                if (couponDiscount.compareTo(COUPON_MAX_DISCOUNT) > 0) {
                    couponDiscount = COUPON_MAX_DISCOUNT;
                }
                totalAfterCoupon = totalAfterProductDiscount.subtract(couponDiscount);
            }
        }

        // Bước 4: Áp dụng Chiết khấu thành viên (Loyalty Discount)
        BigDecimal loyaltyDiscountPercent = BigDecimal.ZERO;
        if (order.getLoyaltyRank() == LoyaltyRank.GOLD) {
            loyaltyDiscountPercent = new BigDecimal("0.05"); // 5%
        } else if (order.getLoyaltyRank() == LoyaltyRank.PLATINUM) {
            loyaltyDiscountPercent = new BigDecimal("0.10"); // 10%
        }
        
        BigDecimal loyaltyDiscountAmount = totalAfterCoupon.multiply(loyaltyDiscountPercent);
        BigDecimal totalAfterLoyalty = totalAfterCoupon.subtract(loyaltyDiscountAmount);

        // Bước 5: Tính Phí vận chuyển (Shipping Fee)
        BigDecimal shippingFee = BigDecimal.ZERO;
        // Kiểm tra điều kiện miễn phí ship dựa trên số tiền sau chiết khấu và trước thuế (> 1,000,000 VND)
        if (totalAfterLoyalty.compareTo(FREE_SHIP_THRESHOLD) <= 0) {
            double distance = order.getShippingDistanceKm();
            if (distance <= BASE_SHIP_DISTANCE_LIMIT) {
                shippingFee = BASE_SHIP_FEE;
            } else {
                double extraDistance = Math.ceil(distance - BASE_SHIP_DISTANCE_LIMIT);
                BigDecimal extraFee = BigDecimal.valueOf(extraDistance).multiply(EXTRA_SHIP_FEE_PER_KM);
                shippingFee = BASE_SHIP_FEE.add(extraFee);
            }
        }

        // Bước 6: Tính Thuế VAT (10% tính trên tổng tiền sau tất cả chiết khấu)
        BigDecimal vatAmount = totalAfterLoyalty.multiply(VAT_RATE);

        // Bước 7: Tính Tổng số tiền cuối cùng (Làm tròn thương mại không lấy phần thập phân lẻ cho VND)
        BigDecimal finalTotal = totalAfterLoyalty.add(vatAmount).add(shippingFee);

        return finalTotal.setScale(0, RoundingMode.HALF_UP);
    }
}