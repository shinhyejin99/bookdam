package kr.or.ddit.dam.log.controller;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;
import kr.or.ddit.dam.admin.service.AdminServiceImpl;
import kr.or.ddit.dam.admin.service.IAdminService;
import kr.or.ddit.dam.cust.service.CustServiceImpl;
import kr.or.ddit.dam.cust.service.ICustService;
import kr.or.ddit.dam.mem.service.IMemService;
import kr.or.ddit.dam.mem.service.MemServiceImpl;
import kr.or.ddit.dam.vo.AdminVO;
import kr.or.ddit.dam.vo.CustVO;
import kr.or.ddit.dam.vo.MemVO;
import kr.or.ddit.dam.util.StreamData;

import java.io.IOException;
import java.io.PrintWriter;

import com.google.gson.Gson;
import com.google.gson.JsonObject;

/**
 * Servlet implementation class Login
 */
@WebServlet("/Login.do")
public class Login extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Login() {
        super();
        // TODO Auto-generated constructor stub
    }

	
	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

	
	 // 클라이언트로부터 JSON 데이터 읽기
    String reqData = StreamData.getChange(request);  
    Gson gson = new Gson();
    JsonObject reqJson = gson.fromJson(reqData, JsonObject.class);
    
    response.setContentType("application/json;charset=utf-8");
    PrintWriter out = response.getWriter();
    JsonObject json = new JsonObject();
    
    // 관리자, 회원 서비스
    IAdminService adminservice = AdminServiceImpl.getService();
    IMemService memservice = MemServiceImpl.getService();
    ICustService custservice = CustServiceImpl.getService();
    
    System.out.println("요청 JSON: " + reqData);

    // 관리자 로그인 처리
    if (reqJson.has("admin_id") && reqJson.has("admin_pass")) {
        String adminId = reqJson.get("admin_id").getAsString();
        String adminPass = reqJson.get("admin_pass").getAsString();

        System.out.println("관리자 로그인 시도: ID=" + adminId + ", PASS=" + adminPass);

        AdminVO admin = adminservice.loginAdmin(adminId, adminPass);

        if (admin != null) {
            // 관리자 로그인 성공
            json.addProperty("flag", "ok");
            json.addProperty("role", "admin");
            json.addProperty("admin_id", admin.getAdmin_id());
            
            // 세션에 관리자 정보 저장
            request.getSession().setAttribute("loginAdmin", admin);
            System.out.println("관리자 로그인 성공");
        } else {
            // 관리자 로그인 실패
            json.addProperty("flag", "fail");
            json.addProperty("message", "입력하신 관리자 정보가 일치하지 않습니다.");
            System.out.println("관리자 로그인 실패");
        }
    } 
    // 일반 회원 로그인 처리
    else if (reqJson.has("mem_mail") && reqJson.has("mem_pass")) {
        String memMail = reqJson.get("mem_mail").getAsString();
        String memPass = reqJson.get("mem_pass").getAsString();

        System.out.println("회원 로그인 시도: MAIL=" + memMail + ", PASS=" + memPass);

        // 회원 로그인 서비스 호출
        MemVO mvo = new MemVO();
        mvo.setMem_mail(memMail);
        mvo.setMem_pass(memPass);
        
        MemVO mvo1 = memservice.loginMember(mvo);

        /// 로그인 성공
        if (mvo1 != null) {
            HttpSession session = request.getSession();

            // 탈퇴된 계정 확인
            String memResignStatus = mvo1.getMem_resign();
            
            // 탈퇴된 계정 처리
            if ("Y".equals(memResignStatus)) {
                json.addProperty("flag", "fail");
                json.addProperty("message", "탈퇴된 계정입니다. 다시 확인해 주세요.");
                System.out.println("탈퇴된 계정으로 로그인 시도");
                
            } else {
                // 로그인 정상 처리
                json.addProperty("flag", "ok");
                json.addProperty("role", "member");
                json.addProperty("mem_id", mvo1.getMem_mail());

                // 세션에 로그인 성공 회원 정보 저장
                request.getSession().setAttribute("loginOk", mvo1);

                // 고객 정보도 가져와서 세션에 저장
                CustVO cvo = custservice.getCustById(mvo1.getMem_mail());
                if (cvo != null) {
                    request.getSession().setAttribute("loginCust", cvo);
                }
                System.out.println("회원 로그인 성공");
            }
        } else {
            json.addProperty("flag", "fail");
            json.addProperty("message", "입력하신 회원 정보가 일치하지 않습니다.");
  
        }
    }

    // 응답 전송
    out.print(json.toString());
    out.flush();
    }
}

